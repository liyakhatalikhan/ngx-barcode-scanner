[![Downloads](http://img.shields.io/npm/dm/@eisberg-labs/ngx-barcode-scanner.svg)](https://npmjs.org/package/@eisberg-labs/ngx-barcode-scanner)
![Build status](https://github.com/eisberg-labs/ngx-barcode-scanner/actions/workflows/ci.yml/badge.svg)
## [ngx-barcode-scanner](https://github.com/eisberg-labs/ngx-barcode-scanner)

Angular 9+ Barcode scanner using [Quagga](https://github.com/ericblade/quagga2).
If you 👍 or use this project, consider giving it a ★, thanks! 🙌  
## [Demo here](https://eisberg-labs.github.io/ngx-barcode-scanner)

## Installation

```sh
$ npm install @eisberg-labs/ngx-barcode-scanner --save
```

## Usage
First import to your module:
```typescript
   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       NgxBarcodeScannerModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }

```
And use in your component html
```html
<ngx-barcode-scanner [(value)]="value" [codes]="['code_128', 'ean', 'upc', 'upc_e', 'ean_8']" [errorThreshold]="0.1" (exception)="onError($event)"></ngx-barcode-scanner>
```
### Start/stop scanning
**ngx-barcode-scanner** ties the scanning service `start` `onInit` and `stop` is `onDestroy`. You may want to control when `start` and `stop` scanner occurs with the help of `NgxBarcodeScannerService`:
```
import {NgxBarcodeScannerService} from "@eisberg-labs/ngx-barcode-scanner";

//...

@Component({
  selector: 'app-root',
  template: `<ngx-barcode-scanner [(value)]="value"
                     [codes]="['code_128', 'ean', 'upc', 'upc_e', 'ean_8']" [errorThreshold]="0.1" (exception)="onError($event)"></ngx-barcode-scanner>`
    <div style="display: flex; justify-content: space-between; margin: 5% auto">
        <button (click)="onStartButtonPress()">Start</button>
        <button (click)="onStopButtonPress()">Stop</button>
    </div>
})

  constructor(
     service: NgxBarcodeScannerService
  ) {
    //Do constructor things...
  }

  onStartButtonPress() {
    this.service.start(this.quaggaConfig, 0.1)
  }

  onValueChanges(detectedValue: string) {
    console.log("Found this: " + detectedValue)
  }
  
  onStopButtonPress() {
    this.service.stop()
  }
```


Supported API
---
### Properties

@Input() | Type | Required|Default|Description
---------|------|---------|-------|-------
codes | string, string[]| required | ['code_128', 'ean', 'ean_8', 'code_39', 'code_39_vin', 'codabar', 'upc', 'upc_e', 'i2of5', '2of5', 'code_93'] | Type of barcode algorithm to detect. Supported are *code_128*,*ean*,*ean_8*,*code_39*,*code_39_vin*,*codabar*,*upc*,*upc_e*,*i2of5*,*2of5*,*code_93*. Be aware that more codes you define, more possible false positives, and it might take longer to detect a barcode.
config | QuaggaJSConfigObject | optional | undefined | Optional [quagga](https://github.com/ericblade/quagga2/blob/253aa01999d0e4a912ca33b119c91fd15cd0294b/type-definitions/quagga.d.ts) config object (Define camera device id, media constraints ...).
errorThreshold | number | optional | 0.1 | Defines threshold of scan detect accuracy. Smaller the value, smaller chance of false positives.
value | string | required | undefined | Scan result outputs to value.

### Events

@Output() | Type | EventType | Required | Description
----------|------|-----------|----------|------------
valueChange | EventEmitter | string | required | Scan result updates
exception | EventEmitter | any | optional | Error events

## For Maintainers
### Releasing a new version 
- Dry run
```cd projects/ngx-barcode-scanner && npx release-it minor --ci --dry-run --no-git.requireUpstream```
- Actual run
```cd projects/ngx-barcode-scanner && npx release-it minor --ci```
- When a new tag is pushed, release workflow should pick it up and publish to npm

## License
MIT © [Eisberg Labs](http://www.eisberg-labs.com)
