# my-goto-js-utils

## 1. To Unix Timestamp
```javascript
export const toUnixTimestamp = (date, seconds=true) => {
  const divider = seconds ? 1000 : 1;
  return (date.getTime() / divider) | 0;
};

```
## 2. Async Wrap
```javascript
export function asyncWrap(promise) {
  return promise.then(result => [null, result]).catch(err => [err]);
}
```
Example Usage
```javascript
const [error, response] = await asyncWrap(searchUsers(search_string));
  if (!error) {
   //DO Something. Look MA No Try catch
  } 
```

## 3.  number_format (PHP Inspired)
```javascript
export const number_format = (val, decimals = 2) => parseFloat(+val).toFixed(decimals);
```
Example usage
```javascript
number_format(totals, 2);
```

## 4. Download Blob
```javascript
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
```javascript
  const { data: response } = await axios({
    url: 'path-to-excel-file',
    responseType: 'blob',
  });
  const blob = window.URL.createObjectURL(new Blob([response]));
  const fileName = `excel.xls`;
  downloadBlob({ blob, fileName });
```

## 5. setTimeoutpromise i.e delay
```javascript
export const delay = (ms = 100) => new Promise(resolve => setTimeout(() => resolve(), ms));
```
Example usage
```javascript
await delay(500);
//do something else after 500 ms delay.
```

## 3. Download File by URL (Browser)
```javascript
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
## 4. Sort On Keys (Secondary Sort)
```javascript
export const sortOnKeys = keysArray => (a, b) => {
  return keysArray.reduce((result, key) => {
    if (result) return result;
    if (a[key] > b[key]) return 1;
    else if (a[key] < b[key]) return -1;
    return 0;
  }, 0);
};
```
Example usage
```javascript
array.sort(sortOnKeys(['propertyA', 'propertyB']))
```
## 5. Formatted Date Display (Browser)
```javascript

  const formattedDate = (d, options) => new Intl.DateTimeFormat('default', options).format(d);
```
Example Usage
```javascript
  const options = {
    hour: 'numeric',
    minute: 'numeric',
    second: 'numeric',
    year: '2-digit',
    hour12: true,
    month: 'numeric',
    day: 'numeric',
  };
console.log(formattedDate(new Date(), options));
```
## 6. Rename Keys of an Object
```javascript
const renameKeys = (keysMap, obj) =>
  Object.keys(obj).reduce(
    (acc, key) => ({
      ...acc,
      ...{ [keysMap[key] || key]: obj[key] },
    }),
    {}
  );
```
Example Usage
```javascript 
renameKeys({productId: 'product_id', productName: 'product_name'}, { productId: 4, productName: 'Keyboard'});
```
## 7. CopyToClipboard (Browser Function)
```javascript
const copyToClipboard = str => {
  const el = document.createElement('textarea');
  el.value = str;
  el.setAttribute('readonly', '');
  el.style.position = 'absolute';
  el.style.left = '-9999px';
  document.body.appendChild(el);
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
};
```
Example usage
```javascript
<button onClick={()=> copyToClipboard("I'm awesome"); }> Copy </button>
```
## 8. IndexArrayBy (Hash by Id)
```javascript
const indexArrayBy = key => array =>
  array.reduce((acc, cur) => {
    acc[cur[key]] = cur;
    return acc;
  }, {});
```
Example usage
```javascript
const arr = [{id: 1, name: 'Github'}, {id: 2, name: 'Gitlab'}, {id: 3, name: 'Bitbucket'}];
const indexBy = indexArrayBy('id');
const arrById = indexBy(arr);

```
## 9. Ceil to nearest multiplier(any decimal <= 1.0)
```javascript
export const ceiling = (num, multiple = 1) => {
  if (multiple === 0 || isNaN(multiple) || isNaN(num)) {
    console.error(ERROR_MESSAGE);
    throw new Error(ERROR_MESSAGE);
  }
  const multiplier = 1 / multiple;
  return Number((Math.ceil(num * multiplier) / multiplier).toFixed(1));
};

```
Example usage
```javascript
ceiling(41.8, 0.5); //Would return 42.0
ceiling(41.23, 0.5); //Would return 41.5
ceiling(41.26, 0.5); //Would return 41.5
```
