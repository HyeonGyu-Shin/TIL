<br>

# ๐ &nbsp; Cloudynary...? ๋ฐฐ์์ผํด!!

<br>

๐ &nbsp; Elice 1์ฐจ ํ๋ก์ ํธ๋ฅผ ์งํ์ค์ด๋ค. ์ด๋ฏธ์ง ์๋ก๋๋ฅผ ์ํด ์ฌ๋ฌ ๋ฐฉ๋ฒ๋ค์ ์์๋ณด๋ ์ค, multer + @๋ก ๊ตฌํํ๋๊ฒ ์ข๋ค๋ ์ฌ์ค์ ์๊ฒ ๋์๋ค.

&nbsp; ์ด์ ๋ฐ๋ผ multer + aws์ s3๋ฅผ ์ฌ์ฉํ  ๊ฒ์ธ์ง, ์๋๋ฉด multer + cloudynary๋ฅผ ์ฌ์ฉํ  ๊ฒ์ธ์ง๋ฅผ ์๊ฐํด๋ดค๋ค.

&nbsp; ๊ทธ ๊ฒฐ๊ณผ ๋์ด๋๊ฐ ์ฌ์ด cloudynary๋ฅผ ์ฌ์ฉํ์๊ณ  ํ๋ค. ๋ฐ๋ผ์ ์ด๋ฒ ์๊ฐ์๋ cloudynary์ ๋ํด ๋ฐฐ์๋ณด์!

<br>

๐ &nbsp; [Cloudynary ๊ณต์๋ฌธ์](https://cloudinary.com/documentation/cloudinary_references)

<br>

# ๐ &nbsp; Cloudynary...? ๊ทธ๊ฒ ๋ญ๋ฐ???

<br>

๐ &nbsp; Cloudynary๋ cloud-based image์ video ๊ด๋ฆฌ ์๋น์ค๋ฅผ ์ ๊ณตํด์ค๋ค.

&nbsp; ์ด๋ website ๋๋ ์ฑ์์ user๊ฐ images์ videos๋ค์ upload, store, manage, manipulate, deliver ํ  ์ ์๊ฒ ํด์ค๋ค.

<br>

# ๐ &nbsp;Cloudynary! ๊ทธ๋ผ ์ฌ์ฉ๋ฒ์ ๋ฐฐ์๋ด์๋ค!!

<br>

<br>

## ๐ &nbsp; 1. express, multer, cloudinary ๋ฐ์์ค๊ธฐ!

<br>

```js

$npm i express multer cloudinary

```

๐ &nbsp; node ํ๊ฒฝ์์ ์ฌ์ฉํ๊ธฐ ์ํด express, multer, cloudinary module์ ๋ฐ์์จ๋ค.

<br>

## ๐ &nbsp; 2. multer์ cloudynary๋ฅผ ์ํ ์ธ์คํด์ค(?) ์์ฑํ๊ธฐ

<br>

```js
// multer.js

const multer = require('multer');

module.exports = multer({
  storage: multer.diskStorage({}),
  limits: { fileSize: 50000 },
});
```

๐ &nbsp; multer.diskStorage() ๋ฉ์๋๋ฅผ ํตํด storage๋ฅผ ์์ฑํด์ฃผ๊ณ , ํ์ํ ์ ํ์ ๊ฑธ์ด์ค ํ export ํ๋ค.

```js
// cloudinary.js

cloudinary.config({
  cloud_name: 'aaa',
  api_key: 'bbb',
  api_secret: 'ccc',
});

module.exports = cloudinary;
```

๐ &nbsp; value ๋ถ๋ถ์ธ 'aaa', 'bbb', 'ccc'์๋ cloudinary's cloud์ cloud_name, API key, API secret ๊ฐ์ ๋ฃ์ด์ฃผ๋ฉด ๋๋ค.

<br>

## ๐ &nbsp; 3. Endpoint์์ upload ์ฒ๋ฆฌ๋ฅผ ํด๋ณด์!

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

๐ &nbsp; post `/upload` ๋ถ๋ถ์ ์ดํด๋ณด์.

&nbsp; form-data๋ก ๋์ด์ค๋ ๋จ์ผ ํ์ผ์ cloudinary์ ์๋ก๋๋ฅผ ํด์ค๋ค. ์ฑ๊ณตํ๋ค๋ฉด json์ผ๋ก ์ฒ๋ฆฌ ๊ฒฐ๊ณผ๋ฅผ ์๋ ค์ค๋ค.

&nbsp; ์ฝ๋๋ฅผ ์คํํ ๊ฒฐ๊ณผ!!

<br>

<img src="./images/image1.png" alt="์ด๋ฏธ์ง1">

<br>

&nbsp; ๋งจ ์ผ์ชฝ์ ๋ด๊ฐ ์๋ก๋ํ ์ฌ์ง์ด ์ ์ฌ๋ผ๊ฐ ๋ชจ์ต์ ํ์ธํ  ์ ์๋ค.

<br>

# ๐ &nbsp; ์ ๊ทธ๋ผ ์ด์  ์ค์ ์ด๋ค! ์ฌ๋ผ๊ฐ ์ด๋ฏธ์ง๋ฅผ ๋ถ๋ฌ์๋ณด์!!

<br>

๐ &nbsp; ๊ณต์๋ฌธ์๋ฅผ ์ฐธ๊ณ ํ์ฌ ์ด๋ฏธ์ง๋ฅผ ๊ฐ์ ธ์ฌ ์ ์๋ search๋ผ๋ ๋ฉ์๋๋ฅผ ์ฐพ์๊ณ  ์ด๋ฅผ ์ด์ฉํด๋ณด๋ ค ํ๋ค.

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

๐ &nbsp; resource_type์ด image์ธ ํ์ผ๋ค์ ๊ฒ์ํด์ ๋ฐํํด์ค๋ค. ์ด๋ฅผ ๊ฐ๊ณตํ์ฌ secure_url์ด ๋ด๊ธด ๋ฐฐ์ด์ ํด๋ผ์ด์ธํธ๋ก ๋ณด๋ด์ค๋ค.

<br>

```html
// index.html

    <button id="btn">์ ์ถ</button>
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

๐ &nbsp; ๋ฐ์์จ ๋ฐฐ์ด์ ์ํํ๋ฉด์ img ํ๊ทธ๋ฅผ ๋ง๋ค๊ณ  img.src์ ๋ฐฐ์ด์ ์์๋ค(url)๋ค์ ๋ฃ์ด์ค ํ ์ด๋ฅผ root์ ๋ ๋๋ง ํด์ค๋ค.

&nbsp; ๊ทธ ๊ฒฐ๊ณผ!!

<br>

<img src="./images/image2.png" alt="์ด๋ฏธ์ง1">

<br>

&nbsp; ์ด๋ฏธ์ง๋ค์ ์ฑ๊ณต์ ์ผ๋ก ๋ถ๋ฌ์ฌ ์ ์์๋ค!!
