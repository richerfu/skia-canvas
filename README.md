# Rust Skia for OpenHarmony

## Build

Install ohrs at first.


```bash
cargo install ohrs
```

And build project.

```bash
ohrs build --arch aarch
```

### For Mac

Make sure that your `ar` support `@response file`. See detail in [here](https://github.com/ninja-build/ninja/issues/2111#issuecomment-1931837345)

You can rewrite ar with llvm-ar

```bash
sudo ln -sf $OHOS_NDK_HOME/native/llvm/bin/llvm-ar /usr/local/bin/ar
```

## Run

Add the following code to your ArkTS file.

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { fileIo as fs } from '@kit.CoreFileKit';
import {draw} from "libskia_canvas.so"

@Entry
@Component
struct Skia {
  @State message: string = 'Hello World';

  saveImage() {
    const context : Context = getContext(this);
    const filePath : string = context.cacheDir + "/test.png";
    let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
    let ret = draw();
    fs.writeSync(file.fd, ret);
  }

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('SkiaHelloWorld')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        }).onClick(()=>{
          this.saveImage();
        })
    }
    .height('100%')
    .width('100%')
  }
}
```