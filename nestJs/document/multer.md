<br>

## ๐ย  Multerโฆ?

<br>

๐ย  file uploading์ ์ํด์ Nest๋ Express์ Multer ๋ฏธ๋ค์จ์ด ๊ธฐ๋ฐ์ Built-in module์ ์ ๊ณตํ๋ค.

์ ๊ทธ๋ผ Multer๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด ๊ด๋ จ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๋ค์ด๋ฐ์.

 <br>

```tsx
npm i -D @types/multer
```

<br>

์์ ์ฝ๋๋ฅผ ๋ค์ด ๋ฐ์๋ค๋ฉด ์ด์  Express.Multer.file type์ ์ฌ์ฉํ  ์ ์๋ค.

<br>

## ๐ย  ์ผ๋ฐ์ ์ธ ์์

<br>

๐ย  ๋จ์ผ ํ์ผ์ ์๋ก๋ ํ๋ ค๋ฉด, ๋จ์ํ FileInterceptor() ์ธํฐ์ํฐ๋ฅผ route handler์ ์ฐ๊ฒฐํ๊ณ , @UploadedFile() ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฌ์ฉํ์ฌ ์์ฒญ์์ ํ์ผ์ ์ถ์ถํ๋ฉด ๋๋ค.

<br>

```tsx
@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(@UploadedFile() file: Express.Multer.File){
	console.log(file);
}
```

<br>

> FileInterceptor() ๋ฐ์ฝ๋ ์ดํฐ๋ @nestjs/platform-express ์์ ๊ฐ์ ธ์ค๊ณ ,
> @UploadedFile() ๋ฐ์ฝ๋ ์ดํฐ๋ @nestjs/common์์ ๊ฐ์ ธ์จ๋ค.

<br>

FileInterceptor() ๋ฐ์ฝ๋ ์ดํฐ๋ ๋ ๊ฐ์ง์ ๋งค๊ฐ๋ณ์๋ฅผ ๊ฐ์ง๋ค.

<br>

- fileName: ํ์ผ์ ๋ณด์ ํ๋ HTML ์์์์ ํ๋ ์ด๋ฆ์ ์ ๊ณตํ๋ ๋ฌธ์์ด
- options: MulterOptions ์ ํ์ optional ๊ฐ์ฒด์ด๋ค. ์ด๊ฑด multer ์์ฑ์๊ฐ ์ฌ์ฉํ๋ ๊ฒ๊ณผ ๋์ผํ ๊ฐ์ฒด์ด๋ค.

<br>

## ๐ย  ํ์ผ ๊ฒ์ฆ

<br>

๐ย  ์ข์ข ํ์ผ ํฌ๊ธฐ ๋๋ ํ์ผ ์ ํ๊ณผ ๊ฐ์ ์์  ํ์ผ ๋ฉํ๋ฐ์ดํฐ์ ์ ํจ์ฑ์ ๊ฒ์ฌํ๋ ๊ฒ์ด ์ ์ฉํ  ์ ์๋ค. ์ด๋ฅผ ์ํด ์์ ๋ง์ pipe๋ฅผ ๋ง๋ค๊ณ , UploadedFile() ๋ฐ์ฝ๋ ์ดํฐ๋ก ๋ฐ์ธ๋ฉํ  ์ ์๋ค.

<br>

```tsx
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class FileSizeValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    // value๋ ํ์ผ์ ์์ฑ๊ณผ ๋ฉํ๋ฐ์ดํฐ๋ฅผ ํฌํจํ๋ ๊ฐ์ฒด์ด๋ค.
    const onekb = 1000;
    return value.size < oneKb;
  }
}
```

<br>

Nest๋ ์ผ๋ฐ์ ์ธ ์ฌ์ฉ ์ฌ๋ก๋ฅผ ์ฒ๋ฆฌํ๊ณ  ์๋ก์ด ์ฌ๋ก ์ถ๊ฐ๋ฅผ ์ฉ์ดํ๊ณ , ํ์คํํ๊ฒ ํด์ฃผ๋ ๋ด์ฅ ํ์ดํ๋ฅผ ์ ๊ณตํด์ค๋ค. ์ด ํ์ดํ๋ ParseFilePipe๋ผ๊ณ  ํ๋ค.

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

๋ณด์๋ค์ํผ ParseFilePipe์ ์ํด ์คํ๋  ํ์ผ ์ ํจ์ฑ ๊ฒ์ฌ๊ธฐ ๋ฐฐ์ด์ ์ง์ ํด์ผ ํ๋ค. ์ฐ๋ฆฌ๋ ์ ํจ์ฑ ๊ฒ์ฌ๊ธฐ์ ์ธํฐํ์ด์ค์ ๋ํด ๋ผ์ํ  ๊ฒ์ด์ง๋ง ๋ ๊ฐ์ง ์ถ๊ฐ ์ต์์ด ์์์ ์ธ๊ธํ  ๊ฐ์น๊ฐ ์๋ค.

<br>

- errorHttpStatusCode โ ์ ํจ์ฑ ๊ฒ์ฌ๊ธฐ๊ฐ ์คํจํ๋ ๊ฒฝ์ฐ ๋ฐ์ํ  HTTP ์ํ ์ฝ๋.
- exceptionFactory โ ์ค๋ฅ ๋ฉ์์ง๋ฅผ ์์ ํ๊ณ  ์ค๋ฅ๋ฅผ ๋ฐํํ๋ ํฉํ ๋ฆฌ.

<br>

**์ ์ด์  FileValidator ์ธํฐํ์ด์ค๋ก ๋์๊ฐ์. ์ ํจ์ฑ ๊ฒ์ฌ๊ธฐ๋ฅผ ์ด ํ์ดํ์ ํตํฉํ๋ ค๋ฉด ๊ธฐ๋ณธ ์ ๊ณต ๊ตฌํ์ ์ฌ์ฉํ๊ฑฐ๋ ๊ณ ์ ํ ์ฌ์ฉ์ ์ง์  FileValidator๋ฅผ ์ ๊ณตํด์ผ ํ๋ค.**

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

**FileValidator๋ ํด๋ผ์ด์ธํธ์์ ์ ๊ณตํ๋ ์ต์์ ๋ฐ๋ผ ํ์ผ ๊ฐ์ฒด์ ์ ๊ทผํ๊ณ  ์ ํจ์ฑ์ ๊ฒ์ฌํ๋ ์ผ๋ฐ ํด๋์ค์ด๋ค. Nest๋ 2๊ฐ์ ๊ธฐ๋ณธ ์ ๊ณต FileValidator ๊ตฌํ์ด ์๋ค.**

<br>

- **MaxFileSizeValidator - ์ฃผ์ด์ง ํ์ผ์ ํฌ๊ธฐ๊ฐ ์ ๊ณต๋ ๊ฐ๋ณด๋ค ์์์ง ํ์ธํ๋ค.**
- **FileTypeValidator - ์ง์ ๋ ํ์ผ์ MIME ์ ํ์ด ์ง์ ๋ ๊ฐ๊ณผ ์ผ์นํ๋์ง ํ์ธํ๋ค.**

<br>

โ Nest๋ ํ์ผ์ ํฌ๊ธฐ์ ์ ํ์ ๊ฒ์ฌํ๋ fileValidator๋ฅผ ์ ๊ณตํ๊ณ , ์ปค์คํํ๊ณ  ์ถ์ผ๋ฉด FileValidator๋ฅผ ํ์ฅํ์ฌ ์ฌ์ฉํ๋ฉด ๋๋ค๋ ๋ป์ธ ๊ฒ ๊ฐ๋ค.

<br>

**์์ ์ธ๊ธํ FIleParsePipe์ ํจ๊ป ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ์ ์ดํดํ๊ธฐ ์ํด ๋ค์ ์๋ฅผ ์ดํด๋ณด์.**

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

> ์ ํจ์ฑ ๊ฒ์ฌ๊ธฐ์ ์๊ฐ ํฌ๊ฒ ์ฆ๊ฐํ๊ฑฐ๋ ์ต์์ด ํ์ผ์ ๋ณต์กํ๊ฒ ๋ง๋๋ ๊ฒฝ์ฐ ๋ณ๋์ ํ์ผ๋ก ๋ถ๋ฆฌํด์ ์ฌ์ฉํด๋ ๋๋ค.

<br>

**๋ง์ง๋ง์ผ๋ก ์ ํจ์ฑ ๊ฒ์ฌ๊ธฐ๋ฅผ ๊ตฌ์ฑํ  ์ ์๋ ํน๋ณํ ParseFilePipeBuilder ํด๋์ค๋ฅผ ์ฌ์ฉํ  ์ ์๋ค.**

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

โ ๊ฒฐ๊ตญ ๋น๋ ํจํด์ ์ด์ฉํ  ์๋ ์๊ณ , ์ธ์คํด์ค ๋ฐฉ์์ ์ด์ฉํ  ์๋ ์๋ค!

<br>

## ๐ย  ์ฌ๋ฌ ๊ฐ์ ํ์ผ

<br>

**๐ย  ํ์ผ ๋ฐฐ์ด์ ์๋ก๋ํ๋ ค๋ฉด FilesInterceptor() ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฌ์ฉํ๋ค. ์ด ๋ฐ์ฝ๋ ์ดํฐ๋ ์ธ ๊ฐ์ง์ ์ธ์๋ฅผ ์ฌ์ฉํ๋ค.**

- **fileName: ํ์ผ์ ๋ณด์ ํ๋ HTML ์์์์ ํ๋ ์ด๋ฆ์ ์ ๊ณตํ๋ ๋ฌธ์์ด**
- **maxCount: ํ์ฉํ  ์ต๋ ํ์ผ์ ์๋ฅผ ์ ์ํ๋ optionalํ ์ซ์**
- **options: MulterOptions ์ ํ์ optional ๊ฐ์ฒด์ด๋ค. ์ด๊ฑด multer ์์ฑ์๊ฐ ์ฌ์ฉํ๋ ๊ฒ๊ณผ ๋์ผํ ๊ฐ์ฒด์ด๋ค.**

  **FilesInterceptor()๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ @UploadedFiles() ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฌ์ฉํ์ฌ ์์ฒญ์์ ํ์ผ์ ์ถ์ถํ๋ค.**

<br>

โ ๋ณต์์ด๊ธฐ ๋๋ฌธ์ ์ธํฐ์ํฐ์ ํ์ดํ ๋ชจ๋์ s๊ฐ ๋ถ๋๋ค! ์ดํดํ๊ธฐ ์ฝ๋ค!!

<br>

```tsx
@Post('upload')
@UseInterceptors(FilesInterceptor('files'))
uploadFile(@UploadedFiles() files: Array<Express.Multer.File>){
	console.log(files);
}
```

<br>

## ๐ย  Multiple files

<br>

**๐ย  ํ๋ ์ด๋ฆ์ด ๋ค๋ฅธ ์ฌ๋ฌ ํ์ผ์ ์๋ก๋ ํ๋ ค๋ฉด FileFieldsInterceptor() ๋ฐ์ฝ๋ ์ดํฐ๋ฅผ ์ฌ์ฉํด์ผ ํ๋ค. ์ด ๋ฐ์ฝ๋ ์ดํฐ๋ ๋ ๊ฐ์ง์ ์ธ์๋ฅผ ์ทจํ๋ค.**

- **uploadedFields โ name๊ณผ maxCount๊ฐ ๋ด๊ธด ๊ฐ์ฒด์ ๋ฐฐ์ด**
- **options: MulterOptions ์ ํ์ optional ๊ฐ์ฒด์ด๋ค. ์ด๊ฑด multer ์์ฑ์๊ฐ ์ฌ์ฉํ๋ ๊ฒ๊ณผ ๋์ผํ ๊ฐ์ฒด์ด๋ค.**

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
