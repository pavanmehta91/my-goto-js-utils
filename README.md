# my-goto-js-utils

## 1. To Unix Timestamp
```
export const toUnixTimestamp = (date, seconds=true) => {
  const divider = seconds ? 1000 : 1;
  return (date.getTime() / divider) | 0;
};

```
## 2. Async Wrap
```
export function asyncWrap(promise) {
  return promise.then(result => [null, result]).catch(err => [err]);
}
```
Example Usage
```
const [error, response] = await asyncWrap(searchUsers(search_string));
  if (!error) {
   //DO Something. Look MA No Try catch
  } 
```

## 3.  number_format (PHP Inspired)
```
export const number_format = (val, decimals = 2) => parseFloat(+val).toFixed(decimals);
```
Example usage
```
number_format(totals, 2);
```

## 4. Download Blob
```
export function downloadBlob({ blob, fileName = 'download' }) {
  const a = document.createElement('a');
  a.href = blob;
  a.style.display = 'none';
  a.download = fileName;
  a.setAttribute('download', fileName);
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  window.URL.revokeObjectURL(blob);
}
```
Example usage
```
                    const { data: response } = await axios({
                      url: 'path-to-excel-file',
                      responseType: 'blob',
                    });
                    const blob = window.URL.createObjectURL(new Blob([response]));
                    const fileName = `excel.xls`;
                    downloadBlob({ blob, fileName });
```

## 5. setTimeoutpromise i.e delay
```
export const delay = (ms = 100) => new Promise(resolve => setTimeout(() => resolve(), ms));
```
Example usage
```
await delay(500);
//do something else after 500 ms delay.
```

## 3. Download File by URL (Browser)
```
export function downloadFile(fileURL, fileName) {
  let save = document.createElement('a');
  save.href = fileURL;
  save.target = '_blank';
  let filename = fileName || fileURL.substring(fileURL.lastIndexOf('/') + 1);
  save.download = filename;
  if (
    navigator.userAgent.toLowerCase().match(/(ipad|iphone|safari)/) &&
    navigator.userAgent.search('Chrome') < 0
  ) {
    document.location = save.href;
    // window event not working here
  } else {
    let evt = new MouseEvent('click', {
      view: window,
      bubbles: true,
      cancelable: false,
    });
    save.dispatchEvent(evt);
    (window.URL || window.webkitURL).revokeObjectURL(save.href);
  }
}
```

