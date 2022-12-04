<br>

## 🔎  Multer…?

<br>

👉  file uploading을 위해서 Nest는 Express의 Multer 미들웨어 기반의 Built-in module을 제공한다.

자 그럼 Multer를 사용하기 위해 관련 라이브러리를 다운받자.

 <br>

```tsx
npm i -D @types/multer
```

<br>

위의 코드를 다운 받았다면 이제 Express.Multer.file type을 사용할 수 있다.

<br>

## 🔎  일반적인 예시

<br>

👉  단일 파일을 업로드 하려면, 단순히 FileInterceptor() 인터셉터를 route handler에 연결하고, @UploadedFile() 데코레이터를 사용하여 요청에서 파일을 추출하면 된다.

<br>

```tsx
@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(@UploadedFile() file: Express.Multer.File){
	console.log(file);
}
```

<br>

> FileInterceptor() 데코레이터는 @nestjs/platform-express 에서 가져오고,
> @UploadedFile() 데코레이터는 @nestjs/common에서 가져온다.

<br>

FileInterceptor() 데코레이터는 두 가지의 매개변수를 가진다.

<br>

- fileName: 파일을 보유하는 HTML 양식에서 필드 이름을 제공하는 문자열
- options: MulterOptions 유형의 optional 객체이다. 이건 multer 생성자가 사용하는 것과 동일한 객체이다.

<br>

## 🔎  파일 검증

<br>

👉  종종 파일 크기 또는 파일 유형과 같은 수신 파일 메타데이터의 유효성을 검사하는 것이 유용할 수 있다. 이를 위해 자신만의 pipe를 만들고, UploadedFile() 데코레이터로 바인딩할 수 있다.

<br>

```tsx
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class FileSizeValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    // value는 파일의 속성과 메타데이터를 포함하는 객체이다.
    const onekb = 1000;
    return value.size < oneKb;
  }
}
```

<br>

Nest는 일반적인 사용 사례를 처리하고 새로운 사례 추가를 용이하고, 표준화하게 해주는 내장 파이프를 제공해준다. 이 파이프는 ParseFilePipe라고 한다.

<br>

```tsx
@Post('file')
uploadFileAndPassValidation(
	@Body() body: SampleDto,
	@UploadedFile(
		new ParseFilePipe({
			validators: []
		})
	) file: Express.Multer.File,
){
	return {
		body,
		file: file.buffer.toString(),
	}
}
```

<br>

보시다시피 ParseFilePipe에 의해 실행될 파일 유효성 검사기 배열을 지정해야 한다. 우리는 유효성 검사기의 인터페이스에 대해 논의할 것이지만 두 가지 추가 옵션이 있음을 언급할 가치가 있다.

<br>

- errorHttpStatusCode → 유효성 검사기가 실패하는 경우 발생할 HTTP 상태 코드.
- exceptionFactory → 오류 메시지를 수신하고 오류를 반환하는 팩토리.

<br>

**자 이제 FileValidator 인터페이스로 돌아가자. 유효성 검사기를 이 파이프와 통합하려면 기본 제공 구현을 사용하거나 고유한 사용자 지정 FileValidator를 제공해야 한다.**

<br>

```tsx
export abstract class FileValidator<TValidationOptions = Record<string, any>> {
  constructor(protected readonly validationOptions: TValidationOptions) {}

  /**
   * Indicates if this file should be considered valid, according to the options passed in the constructor.
   * @param file the file from the request object
   */
  abstract isValid(file?: any): boolean | Promise<boolean>;

  /**
   * Builds an error message in case the validation fails.
   * @param file the file from the request object
   */
  abstract buildErrorMessage(file: any): string;
}
```

<br>

**FileValidator는 클라이언트에서 제공하는 옵션에 따라 파일 객체에 접근하고 유효성을 검사하는 일반 클래스이다. Nest는 2개의 기본 제공 FileValidator 구현이 있다.**

<br>

- **MaxFileSizeValidator - 주어진 파일의 크기가 제공된 값보다 작은지 확인한다.**
- **FileTypeValidator - 지정된 파일의 MIME 유형이 지정된 값과 일치하는지 확인한다.**

<br>

→ Nest는 파일의 크기와 유형을 검사하는 fileValidator를 제공하고, 커스텀하고 싶으면 FileValidator를 확장하여 사용하면 된다는 뜻인 것 같다.

<br>

**앞서 언급한 FIleParsePipe와 함께 사용하는 방법을 이해하기 위해 다음 예를 살펴보자.**

<br>

```tsx
@UploadedFile(
	new ParseFilePipe({
		validators: [
			new MaxFileSizeValidator({maxSize: 1000}),
			new FileTypeValidator({fileType: 'jpeg'}),
		]
	})
) file: Express.Multer.File,
```

<br>

> 유효성 검사기의 수가 크게 증가하거나 옵션이 파일을 복잡하게 만드는 경우 별도의 파일로 분리해서 사용해도 된다.

<br>

**마지막으로 유효성 검사기를 구성할 수 있는 특별한 ParseFilePipeBuilder 클래스를 사용할 수 있다.**

<br>

```tsx
@UploadedFile(
	new ParseFilePipeBuilder()
		.addFileTypeValidator({fileType:'jpeg'})
		.addMaxSizeValidator({maxSize: 1000})
		.build({errorHttpStatusCode: HttpStatus.UNPROCESSABLE_ENTITY})
) file: Express.Multer.File
```

<br>

→ 결국 빌더 패턴을 이용할 수도 있고, 인스턴스 방식을 이용할 수도 있다!

<br>

## 🔎  여러 개의 파일

<br>

**👉  파일 배열을 업로드하려면 FilesInterceptor() 데코레이터를 사용한다. 이 데코레이터는 세 가지의 인수를 사용한다.**

- **fileName: 파일을 보유하는 HTML 양식에서 필드 이름을 제공하는 문자열**
- **maxCount: 허용할 최대 파일의 수를 정의하는 optional한 숫자**
- **options: MulterOptions 유형의 optional 객체이다. 이건 multer 생성자가 사용하는 것과 동일한 객체이다.**

  **FilesInterceptor()를 사용하는 경우 @UploadedFiles() 데코레이터를 사용하여 요청에서 파일을 추출한다.**

<br>

→ 복수이기 때문에 인터셉터와 파이프 모두에 s가 붙는다! 이해하기 쉽네!!

<br>

```tsx
@Post('upload')
@UseInterceptors(FilesInterceptor('files'))
uploadFile(@UploadedFiles() files: Array<Express.Multer.File>){
	console.log(files);
}
```

<br>

## 🔎  Multiple files

<br>

**👉  필드 이름이 다른 여러 파일을 업로드 하려면 FileFieldsInterceptor() 데코레이터를 사용해야 한다. 이 데코레이터는 두 가지의 인수를 취한다.**

- **uploadedFields → name과 maxCount가 담긴 객체의 배열**
- **options: MulterOptions 유형의 optional 객체이다. 이건 multer 생성자가 사용하는 것과 동일한 객체이다.**

<br>

```tsx
@Post('upload')
@UseInterceptors(FileFieldsInterceptor([
	{name: 'avatar', maxCount: 1},
	{name: 'background', maxCOunt: 1},
]))
uploadFile(@UploadedFiles() files: {avatar?: Express.Multer.File[], background?: Express.Multer.File[]}){
	console.log(files);
}
```
