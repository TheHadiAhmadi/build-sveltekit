# Build sveltekit based projects (only static)

To build sveltekit based projects, first we should install static adapter.

```
npm i @sveltejs/adapter-static
```
Then edit svelte.config.js file: 
```diff
Import "@sveltejs/adapter-auto"
Import "@sveltejs/adapter-static"
```

Then we need to decide between SPA and SSR modes

## SPA: 
To build SPA sites we should disable prerender and add fallback option for static adapter:

 ```js
import adapter from '@sveltejs/adapter-static';
 
export default {
 kit: {
   adapter: adapter({
     fallback: "index.html"
   }),
   prerender: { entries: [] }
 }
};

```
Also we can disable prerender and ssr using +layout.ts files

// src/routes/+layout.ts
```
Export const prerender = false
Export const ssr = false
```

Then after build an `index.html` file will be created which is SPA and doesn't support ssr.

اگر قرار است پروژه ssr باشد دو حالت وجود دارد. یا محتویات صفحات در وقت build مشخص است یا مشخص نیست و دیتا بصورت داینامیک است و هر زمان ممکن است کم یا زیاد شود. 
در حالت دوم با استفاده static-adapter ممکن نیست و باید از آداپتر دیگه مثل @sveltejs/adapter-node استفاده کنیم.
در حالت اول که محتویات صفحه در وقت build معلوم است باید prerender را برای صفحاتی که میخواهیم ssr باشد فعال کنیم.


src/routes/+layout.ts
```
export const ssr = true
export const prerender = true
```
و در svelte.config.js نباید fallback را تعیین کنیم.

در نتیجه بعد از build برای هر route یک فایل html جنریت میشه. 

## trailingSlash:
برای مثال صفحه src/routes/about/+page.svelte را در نظر بگیرید. برای جنریت شدن صفحه دو حالت وجود دارد.
about/index.html
about.html


برای این که تعیین کنیم کدام فایل بالا جنریت شود از آپشن trailingSlash استفاده میکنیم.
اگرtrailingSlash = always باشد فایل about/index.html ساخته میشود 
site.com/about/


و اگر trailingSlash = never باشد فایل about.html ساخته میشود.
site.com/about.html
(حالت دوم در بعضی از hosting provider ها کار نمیدهد و مجبور میشیم از always استفاده کنیم.)



