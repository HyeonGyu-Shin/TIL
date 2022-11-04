<br>

# 📌 &nbsp; Cloudynary...? 배워야해!!

<br>

👉 &nbsp; Elice 1차 프로젝트를 진행중이다. 이미지 업로드를 위해 여러 방법들을 알아보던 중, multer + @로 구현하는게 좋다는 사실을 알게 되었다.

&nbsp; 이에 따라 multer + aws의 s3를 사용할 것인지, 아니면 multer + cloudynary를 사용할 것인지를 생각해봤다.

&nbsp; 그 결과 난이도가 쉬운 cloudynary를 사용하자고 했다. 따라서 이번 시간에는 cloudynary에 대해 배워보자!

<br>

👉 &nbsp; [Cloudynary 공식문서](https://cloudinary.com/documentation/cloudinary_references)

<br>

# 📌 &nbsp; Cloudynary...? 그게 뭔데???

<br>

👉 &nbsp; Cloudynary는 cloud-based image와 video 관리 서비스를 제공해준다.

&nbsp; 이는 website 또는 앱에서 user가 images와 videos들을 upload, store, manage, manipulate, deliver 할 수 있게 해준다.

<br>

# 📌 &nbsp;Cloudynary! 그럼 사용법을 배워봅시다!!

<br>

<br>

## 🔎 &nbsp; 1. express, multer, cloudinary 받아오기!

<br>

```js

$npm i express multer cloudinary

```

👉 &nbsp; node 환경에서 사용하기 위해 express, multer, cloudinary module을 받아온다.

<br>

## 🔎 &nbsp; 2. multer와 cloudynary를 위한 인스턴스(?) 생성하기

<br>

```js
// multer.js

const multer = require('multer');

module.exports = multer({
  storage: multer.diskStorage({}),
  limits: { fileSize: 50000 },
});
```

👉 &nbsp; multer.diskStorage() 메서드를 통해 storage를 생성해주고, 필요한 제한을 걸어준 후 export 한다.

```js
// cloudinary.js

cloudinary.config({
  cloud_name: 'aaa',
  api_key: 'bbb',
  api_secret: 'ccc',
});

module.exports = cloudinary;
```

👉 &nbsp; value 부분인 'aaa', 'bbb', 'ccc'에는 cloudinary's cloud의 cloud_name, API key, API secret 값을 넣어주면 된다.

<br>

## 🔎 &nbsp; 3. Endpoint에서 upload 처리를 해보자!

<br>

```js
// app.js
const express = require('express');
const cloudynary = require('./cloudynary');
const uploader = require('./multer');

const app = express();

app.get('/', (req, res) => {
  res.send('Server is running');
});

app.post('/upload', uploader.single('file'), async (req, res) => {
  const upload = await cloudinary.v2.uploader.upload(req.file.path);
  return res.json({
    success: true,
    file: upload.secure_url,
  });
});

app.listen(3000);
```

👉 &nbsp; post `/upload` 부분을 살펴보자.

&nbsp; form-data로 넘어오는 단일 파일을 cloudinary에 업로드를 해준다. 성공한다면 json으로 처리 결과를 알려준다.

&nbsp; 코드를 실행한 결과!!

<br>

<img src="./images/image1.png" alt="이미지1">

<br>

&nbsp; 맨 왼쪽에 내가 업로드한 사진이 잘 올라간 모습을 확인할 수 있다.

<br>

# 📌 &nbsp; 자 그럼 이제 실전이다! 올라간 이미지를 불러와보자!!

<br>

👉 &nbsp; 공식문서를 참고하여 이미지를 가져올 수 있는 search라는 메서드를 찾았고 이를 이용해보려 한다.

<br>

```js
// app.js

...

app.post('/upload', async (req, res) => {
  const result = await cloudinary.v2.search
    .expression('resource_type: image')
    .execute();

  const filtered = result.resources.map((v) => v.secure_url);

  return res.json(filtered);

});

...

```

<br>

👉 &nbsp; resource_type이 image인 파일들을 검색해서 반환해준다. 이를 가공하여 secure_url이 담긴 배열을 클라이언트로 보내준다.

<br>

```html
// index.html

    <button id="btn">제출</button>
    <div id="root" style="display: flex"></div>

    <script>
        const btn = document.querySelector('#btn');
        const root = document.querySelector('#root');

        btn.addEventListener('click', async (e) => {
            e.preventDefault();
            const result = await fetch('http://localhost:5500/upload', {
                method: 'POST'
            }).then(res => res.json())

            result.forEach(v => {
                const img = document.createElement('img');
                img.src = v;
                img.style.width = '200px';
                img.style.height = '200px';
                img.style.backgroundSize = 'cover';
                root.appendChild(img);
            })
        })

```

<br>

👉 &nbsp; 받아온 배열을 순회하면서 img 태그를 만들고 img.src에 배열의 요소들(url)들을 넣어준 후 이를 root에 렌더링 해준다.

&nbsp; 그 결과!!

<br>

<img src="./images/image2.png" alt="이미지1">

<br>

&nbsp; 이미지들을 성공적으로 불러올 수 있었다!!
