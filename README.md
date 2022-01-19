# Image widget with an auto WebP file generation for Yii2 Framework

**ImgOpt** is an image optimization widget for [Yii2 Framework](https://www.yiiframework.com) with auto [WebP](https://developers.google.com/speed/webp) image format generation from `PNG` and `JPG` files.

## How to make my website faster?

[My website](https://www.pelock.com) had all the beautiful images and screenshots, but there was one problem. Most of them were in `PNG` format, some of them weighted around 200 kB. And it adds up to the point where my website loading time was just slow.

I found about the WebP format, read that it's supported in the latest browsers and if it's not (only older Safari browsers), there's a way to overcome this and serve the default `PNG` or `JPG` images. Perfect.

_But_ the entire process would require me to go manually and use some sort of image conversion tool, upload new WebP images to the server and upgrade my `HTML` code.

To hell with that! We can do better!

## Automate PNG & JPG to WebP conversion

I have decided to create a Yii2 widget that would automate this task.

What it does? Instead of static images like this:

```html
<img src="/images/product/extra.png" alt="Extra product">
```

it will automatically generate an extra image in new [WebP](https://developers.google.com/speed/webp) format (in the same directory, where the provided image is located) and serve it to your browser in HTML `<picture>` tag, with a default fallback to the original image for browsers that don't support WebP images.

Replace your `IMG` tag within your `HTML` templates with a call to:

```php
<?= \PELock\ImgOpt\ImgOpt::widget(["src" => "/images/product/extra.png", "alt" => "Extra product" ]) ?>
```

(Image path is relative to [Yii2 Framework @webroot alias](https://www.yiiframework.com/wiki/667/yii-2-list-of-path-aliases-available-with-default-basic-and-advanced-app))

And once run, the widget code will generate a new WebP image file on the fly (original image is left **untouched**) and he following HTML code gets generated:

```html
<picture>
    <source type="image/webp" srcset="/images/product/extra.webp">
    <img src="/images/product/extra.png" alt="Extra product">
</picture>
```

The browser will pick up the best source for the provided image, and thanks to revolutionary WebP compression, it will make your website loading faster.

## Image lazy-loading

[Lazy images loading](https://web.dev/browser-level-image-lazy-loading/) makes the browser load the images when it reach a certain point, after which the image became visible in the current browser tab. You can use this pure HTML feature (no JS involved) from within the widget:

```php
<?= \PELock\ImgOpt\ImgOpt::widget(["src" => "/images/product/extra.png", "loading" => "lazy" ]) ?>
```

The generated output looks like this:

```html
<picture>
    <source type="image/webp" srcset="/images/product/extra.webp">
    <img src="/images/product/extra.png" loading="lazy">
</picture>
```

Use it to make your website loading times even faster.

## Automatic WebP generation for updated images (new in v1.2.0)

ImgOpt will set the modification date of the generated WebP image to match the modification date of the original image file.

If ImgOpt detects that a file modification date of the source image file is different than the date of the previously generated WebP image file - it will automatically re-create the new WebP image file!

## Installation

The preferred way to install the library is through the [composer](https://getcomposer.org/).

Run:

```
php composer.phar require --prefer-dist pelock/yii2-imgopt "*"
```

Or add:

```
"pelock/yii2-imgopt": "*"
```

to the`require` section within your `composer.json` config file.

The installation package is available at https://packagist.org/packages/pelock/yii2-imgopt

The Yii2 extension is available at https://www.yiiframework.com/extension/pelock/yii2-imgopt

Source code is available at https://github.com/PELock/yii2-imgopt

## Image quality

I knew you would ask about it! By default the conversion tries all the steps from 100% output image quality down to 70% to generate the WebP file that is smaller than the original image.

| Original PNG (181 kB) | Optimized WebP (60 kB) |
| --------------------- | -------------- |
| [![Social Media Bot](https://www.pelock.com/img/media_social_bot.png)](https://www.pelock.com/products/social-media-bot) | [![Social Media Bot](https://www.pelock.com/img/media_social_bot.webp)](https://www.pelock.com/products/social-media-bot/install) |

If the generated WebP image is larger than the original image, the default `<img>` tag will be generated.

## Disable WebP images serving

If for some reason you want to disable WebP file serving via the HTML `<picture>` tag, you can do it per widget settings:

```php
<?= \PELock\ImgOpt\ImgOpt::widget(["src" => "/images/product/extra.png", "alt" => "Extra product", "disable" => true ]) ?>
```

## Recreate WebP file

The widget code automatically detects if there's a WebP image in the directory with the original image. If it's not there - it will recreate it. It's only done once.

If you wish to force the widget code to recreate it anyway, pass the special param to the widget code:

```php
<?= \PELock\ImgOpt\ImgOpt::widget(["src" => "/images/product/extra.png", "alt" => "Extra product", "recreate" => true ]) ?>
```

You might want to recreate all of the WebP files and to do that without modifying, change the widget source code from:

```php
/**
 * @var bool set to TRUE to recreate *ALL* of the WebP files again (optional)
 */
const RECREATE_ALL = false;
```

to:

```php
/**
 * @var bool set to TRUE to recreate *ALL* of the WebP files again (optional)
 */
const RECREATE_ALL = true;
```


## Lightbox 2 integration

You can also generate Lightbox (https://lokeshdhakar.com/projects/lightbox2/) friendly images.

Instead of:

```html
<a href="/images/sunset.jpg" data-lightbox="image-1" data-title="Sunset">
    <img src="/images/sunset-thumbnail.jpg" alt="Sunset">
</a>
```

You can replace it with more compact widget code:

```php
<?= \PELock\ImgOpt\ImgOpt::widget(["lightbox_data" => "image-1", "lightbox_src" => "/images/sunset.jpg", "src" => "/images/sunset-thumbnail.jpg", "alt" => "Sunset" ]) ?>
```

And it will generate this HTML code:

```html
<a href="/images/sunset.jpg" data-lightbox="image-1" data-title="Sunset">
    <picture>
        <source type="image/webp" srcset="/images/sunset-thumbnail.webp">
        <img src="/images/sunset-thumbnail.png" alt="Sunset">
    </picture>
</a>
```

## Bugs, questions, feature requests

Hope you like it. For questions, bug & feature requests visit my site:

Bartosz Wójcik | https://www.pelock.com