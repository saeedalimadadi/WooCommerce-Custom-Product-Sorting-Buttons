

# WooCommerce Custom Product Sorting Buttons

This code snippet provides a **shortcode to display custom sorting buttons for your WooCommerce products**. By default, WooCommerce offers a dropdown for product sorting. This solution allows you to replace or augment that dropdown with a set of visually distinct buttons, making it easier and more intuitive for your customers to sort products by popularity, rating, date, or price (ascending/descending).

## Why use custom sorting buttons?

* **Improved User Experience:** Buttons are often more discoverable and easier to interact with than dropdowns, especially on mobile devices.
* **Enhanced Aesthetics:** Integrate sorting options seamlessly into your theme's design with custom styling.
* **Clearer Options:** Present all sorting options at a glance, reducing clicks for users.
* **Customization:** Full control over the text and order of sorting options.

## Features

* Provides a shortcode `[custom_sorting_buttons]` that outputs HTML buttons.
* Each button links to a specific WooCommerce sorting parameter (`orderby`).
* Includes options for popularity, average rating, latest products, price low-to-high, and price high-to-low.

## Installation

There are two primary methods to implement this code:

### Method 1: As a Standalone Plugin (Recommended)

Creating a small, dedicated plugin is the most robust way to add this functionality. It ensures your code remains active regardless of theme changes.

1.  Create a new folder named `wc-custom-sort-buttons` inside your WordPress site's `wp-content/plugins/` directory.
2.  Inside this new folder, create a file named `wc-custom-sort-buttons.php`.
3.  Copy and paste the following code into `wc-custom-sort-buttons.php`:

    ```php
    <?php
    /**
     * Plugin Name: WooCommerce Custom Product Sorting Buttons
     * Description: Adds a shortcode to display custom sorting buttons for WooCommerce products.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     * Text Domain: wc-custom-sort-buttons
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    /**
     * Shortcode to display custom product sorting buttons.
     * Use [custom_sorting_buttons] in your pages or theme files.
     */
    function custom_sorting_buttons_shortcode() {
        ob_start();
        ?>
        <div class="custom-sorting-buttons">
            <a href="?orderby=popularity" class="sorting-button">Popularity</a>
            <a href="?orderby=rating" class="sorting-button">Average rating</a>
            <a href="?orderby=date" class="sorting-button">Newest</a>
            <a href="?orderby=price" class="sorting-button">Price low &uarr;</a>
            <a href="?orderby=price-desc" class="sorting-button">Price high &darr;</a>
        </div>
        <?php
        return ob_get_clean();
    }
    add_shortcode('custom_sorting_buttons', 'custom_sorting_buttons_shortcode');

    /**
     * Optionally add basic styling for the buttons.
     * You should ideally style these buttons in your theme's stylesheet.
     */
    function custom_sorting_buttons_styles() {
        if ( is_shop() || is_product_category() || is_product_tag() || is_product_taxonomy() ) {
            ?>
            <style type="text/css">
                .custom-sorting-buttons {
                    display: flex;
                    gap: 10px;
                    margin-bottom: 20px;
                    flex-wrap: wrap; /* Allows buttons to wrap on smaller screens */
                }
                .custom-sorting-buttons .sorting-button {
                    background-color: #f0f0f0;
                    border: 1px solid #ccc;
                    padding: 8px 15px;
                    text-decoration: none;
                    color: #333;
                    border-radius: 5px;
                    transition: all 0.2s ease-in-out;
                    white-space: nowrap; /* Prevent text wrapping within button */
                }
                .custom-sorting-buttons .sorting-button:hover {
                    background-color: #e0e0e0;
                    color: #000;
                }
                .custom-sorting-buttons .sorting-button.active {
                    background-color: #0073aa; /* WordPress blue */
                    color: #fff;
                    border-color: #0073aa;
                }
                /* Basic responsive adjustments */
                @media (max-width: 600px) {
                    .custom-sorting-buttons {
                        justify-content: center;
                    }
                }
            </style>
            <script type="text/javascript">
                document.addEventListener('DOMContentLoaded', function() {
                    const urlParams = new URLSearchParams(window.location.search);
                    const orderBy = urlParams.get('orderby');
                    const buttons = document.querySelectorAll('.custom-sorting-buttons .sorting-button');
                    buttons.forEach(button => {
                        const buttonOrderBy = new URLSearchParams(button.href.split('?')[1]).get('orderby');
                        if (orderBy === buttonOrderBy) {
                            button.classList.add('active');
                        }
                    });
                });
            </script>
            <?php
        }
    }
    add_action('wp_head', 'custom_sorting_buttons_styles');
    ```

4.  Go to your WordPress admin dashboard, navigate to **"Plugins"**, and **activate** the "WooCommerce Custom Product Sorting Buttons" plugin.

### Method 2: Adding to your Theme's functions.php File

You can add this code directly to your active theme's `functions.php` file. **Before doing so, it's highly recommended to back up your `functions.php` file.**

1.  Navigate to `wp-content/themes/YourThemeName/` (replace `YourThemeName` with the actual name of your active theme).
2.  Open the `functions.php` file.
3.  Add the following code to the end of the file (before the closing `?>` tag, if one exists):

    ```php
    /**
     * Shortcode to display custom product sorting buttons.
     * Use [custom_sorting_buttons] in your pages or theme files.
     */
    function custom_sorting_buttons_shortcode() {
        ob_start();
        ?>
        <div class="custom-sorting-buttons">
            <a href="?orderby=popularity" class="sorting-button">Popularity</a>
            <a href="?orderby=rating" class="sorting-button">Average rating</a>
            <a href="?orderby=date" class="sorting-button">Newest</a>
            <a href="?orderby=price" class="sorting-button">Price low &uarr;</a>
            <a href="?orderby=price-desc" class="sorting-button">Price high &darr;</a>
        </div>
        <?php
        return ob_get_clean();
    }
    add_shortcode('custom_sorting_buttons', 'custom_sorting_buttons_shortcode');

    /**
     * Optionally add basic styling for the buttons.
     * You should ideally style these buttons in your theme's stylesheet.
     */
    function custom_sorting_buttons_styles() {
        if ( is_shop() || is_product_category() || is_product_tag() || is_product_taxonomy() ) {
            ?>
            <style type="text/css">
                .custom-sorting-buttons {
                    display: flex;
                    gap: 10px;
                    margin-bottom: 20px;
                    flex-wrap: wrap; /* Allows buttons to wrap on smaller screens */
                }
                .custom-sorting-buttons .sorting-button {
                    background-color: #f0f0f0;
                    border: 1px solid #ccc;
                    padding: 8px 15px;
                    text-decoration: none;
                    color: #333;
                    border-radius: 5px;
                    transition: all 0.2s ease-in-out;
                    white-space: nowrap; /* Prevent text wrapping within button */
                }
                .custom-sorting-buttons .sorting-button:hover {
                    background-color: #e0e0e0;
                    color: #000;
                }
                .custom-sorting-buttons .sorting-button.active {
                    background-color: #0073aa; /* WordPress blue */
                    color: #fff;
                    border-color: #0073aa;
                }
                /* Basic responsive adjustments */
                @media (max-width: 600px) {
                    .custom-sorting-buttons {
                        justify-content: center;
                    }
                }
            </style>
            <script type="text/javascript">
                document.addEventListener('DOMContentLoaded', function() {
                    const urlParams = new URLSearchParams(window.location.search);
                    const orderBy = urlParams.get('orderby');
                    const buttons = document.querySelectorAll('.custom-sorting-buttons .sorting-button');
                    buttons.forEach(button => {
                        const buttonOrderBy = new URLSearchParams(button.href.split('?')[1]).get('orderby');
                        if (orderBy === buttonOrderBy) {
                            button.classList.add('active');
                        }
                    });
                });
            </script>
            <?php
        }
    }
    add_action('wp_head', 'custom_sorting_buttons_styles');
    ```

## Usage

Once installed, simply add the shortcode `[custom_sorting_buttons]` to any page, post, or text widget where you want the sorting buttons to appear. This is typically placed above the WooCommerce product loop on shop or archive pages.

**Example for placement in theme files:**

```php
<?php echo do_shortcode('[custom_sorting_buttons]'); ?>
```
Customization
Styling: The provided code includes basic inline CSS for the buttons. For more robust and theme-integrated styling, it's highly recommended to move the CSS from the custom_sorting_buttons_styles function into your theme's style.css file or a dedicated CSS file. You can then modify the .custom-sorting-buttons and .sorting-button classes to match your site's design.

Sorting Options: You can modify the <a> tags within the custom_sorting_buttons_shortcode function to change the text or add/remove sorting options. Ensure the orderby parameter in the href attribute matches valid WooCommerce sorting options (e.g., menu_order, popularity, rating, date, price, price-desc, title).

Active Button Highlight: The included JavaScript dynamically adds an active class to the currently selected sorting button. You can style this .active class in your CSS to highlight the active button.

Contributing
Contributions are welcome! If you have suggestions or improvements for this code, feel free to open a "Pull Request" or report an "Issue."

License
This project is licensed under the GPL-2.0-or-later License.


---


# دکمه‌های سفارشی مرتب‌سازی محصولات ووکامرس

این قطعه کد یک **شورت‌کد برای نمایش دکمه‌های سفارشی مرتب‌سازی محصولات ووکامرس** ارائه می‌دهد. به طور پیش‌فرض، ووکامرس یک منوی کشویی برای مرتب‌سازی محصولات دارد. این راه حل به شما اجازه می‌دهد تا آن منو را با مجموعه‌ای از دکمه‌های بصری متمایز جایگزین یا تکمیل کنید، که مرتب‌سازی محصولات بر اساس محبوبیت، امتیاز، تاریخ یا قیمت (صعودی/نزولی) را برای مشتریان شما آسان‌تر و بصری‌تر می‌کند.

## چرا از دکمه‌های سفارشی مرتب‌سازی استفاده کنیم؟

* **بهبود تجربه کاربری:** دکمه‌ها اغلب قابل کشف‌تر و آسان‌تر برای تعامل هستند تا منوهای کشویی، به خصوص در دستگاه‌های تلفن همراه.
* **زیبایی‌شناسی پیشرفته:** گزینه‌های مرتب‌سازی را به طور یکپارچه با طراحی قالب شما و با استایل‌دهی سفارشی ادغام کنید.
* **گزینه‌های واضح‌تر:** تمام گزینه‌های مرتب‌سازی را در یک نگاه ارائه می‌دهد و تعداد کلیک‌ها را برای کاربران کاهش می‌دهد.
* **قابلیت سفارشی‌سازی:** کنترل کامل بر متن و ترتیب گزینه‌های مرتب‌سازی.

## قابلیت‌ها

* ارائه یک شورت‌کد `[custom_sorting_buttons]` که دکمه‌های HTML را خروجی می‌دهد.
* هر دکمه به یک پارامتر مرتب‌سازی خاص ووکامرس (`orderby`) لینک می‌دهد.
* شامل گزینه‌هایی برای محبوبیت، میانگین امتیاز، جدیدترین محصولات، قیمت از کم به زیاد، و قیمت از زیاد به کم.

## نصب

برای پیاده‌سازی این کد، دو روش اصلی وجود دارد:

### روش ۱: به عنوان یک افزونه مستقل (توصیه شده)

ایجاد یک افزونه کوچک و اختصاصی، قوی‌ترین راه برای افزودن این قابلیت است. این تضمین می‌کند که کد شما صرف‌نظر از تغییر قالب، فعال باقی بماند.

1.  یک پوشه جدید با نام `wc-custom-sort-buttons` در مسیر `wp-content/plugins/` سایت وردپرسی خود ایجاد کنید.
2.  در داخل این پوشه جدید، یک فایل با نام `wc-custom-sort-buttons.php` ایجاد کنید.
3.  کد زیر را در `wc-custom-sort-buttons.php` کپی و جایگذاری کنید:

    ```php
    <?php
    /**
     * Plugin Name: WooCommerce Custom Product Sorting Buttons
     * Description: Adds a shortcode to display custom sorting buttons for WooCommerce products.
     * Version: 1.0
     * Author: Your Name (Optional)
     * License: GPL-2.0-or-later
     * Text Domain: wc-custom-sort-buttons
     */

    if ( ! defined( 'ABSPATH' ) ) {
        exit; // Exit if accessed directly.
    }

    /**
     * Shortcode to display custom product sorting buttons.
     * Use [custom_sorting_buttons] in your pages or theme files.
     */
    function custom_sorting_buttons_shortcode() {
        ob_start();
        ?>
        <div class="custom-sorting-buttons">
            <a href="?orderby=popularity" class="sorting-button">محبوب</a>
            <a href="?orderby=rating" class="sorting-button">امتیاز</a>
            <a href="?orderby=date" class="sorting-button">جدید</a>
            <a href="?orderby=price" class="sorting-button">قیمت &uarr;</a>
            <a href="?orderby=price-desc" class="sorting-button">قیمت &darr;</a>
        </div>
        <?php
        return ob_get_clean();
    }
    add_shortcode('custom_sorting_buttons', 'custom_sorting_buttons_shortcode');

    /**
     * Optionally add basic styling for the buttons.
     * You should ideally style these buttons in your theme's stylesheet.
     */
    function custom_sorting_buttons_styles() {
        if ( is_shop() || is_product_category() || is_product_tag() || is_product_taxonomy() ) {
            ?>
            <style type="text/css">
                .custom-sorting-buttons {
                    display: flex;
                    gap: 10px;
                    margin-bottom: 20px;
                    flex-wrap: wrap; /* اجازه می‌دهد دکمه‌ها در صفحه‌های کوچک‌تر به خط بعدی بروند */
                    justify-content: flex-start; /* شروع از سمت چپ */
                }
                .custom-sorting-buttons .sorting-button {
                    background-color: #f0f0f0;
                    border: 1px solid #ccc;
                    padding: 8px 15px;
                    text-decoration: none;
                    color: #333;
                    border-radius: 5px;
                    transition: all 0.2s ease-in-out;
                    white-space: nowrap; /* جلوگیری از شکستن متن درون دکمه */
                }
                .custom-sorting-buttons .sorting-button:hover {
                    background-color: #e0e0e0;
                    color: #000;
                }
                .custom-sorting-buttons .sorting-button.active {
                    background-color: #0073aa; /* آبی وردپرس */
                    color: #fff;
                    border-color: #0073aa;
                }
                /* تنظیمات ریسپانسیو پایه */
                @media (max-width: 600px) {
                    .custom-sorting-buttons {
                        justify-content: center; /* در صفحه‌های کوچک‌تر دکمه‌ها را وسط چین می‌کند */
                    }
                }
            </style>
            <script type="text/javascript">
                document.addEventListener('DOMContentLoaded', function() {
                    const urlParams = new URLSearchParams(window.location.search);
                    const orderBy = urlParams.get('orderby');
                    const buttons = document.querySelectorAll('.custom-sorting-buttons .sorting-button');
                    buttons.forEach(button => {
                        const buttonHref = button.href;
                        // Extract query string from the button's href
                        const queryString = buttonHref.includes('?') ? buttonHref.split('?')[1] : '';
                        const buttonUrlParams = new URLSearchParams(queryString);
                        const buttonOrderBy = buttonUrlParams.get('orderby');

                        if (orderBy === buttonOrderBy) {
                            button.classList.add('active');
                        }
                    });
                });
            </script>
            <?php
        }
    }
    add_action('wp_head', 'custom_sorting_buttons_styles');
    ```

4.  وارد پنل مدیریت وردپرس خود شوید، به بخش **"افزونه‌ها"** بروید و افزونه **"دکمه‌های سفارشی مرتب‌سازی محصولات ووکامرس"** را **فعال کنید**.

### روش ۲: اضافه کردن به فایل functions.php قالب شما

می‌توانید این کد را مستقیماً به فایل `functions.php` قالب فعال خود اضافه کنید. **پیشنهاد اکید می‌شود قبل از انجام این کار، از فایل `functions.php` خود یک پشتیبان (backup) تهیه کنید.**

1.  به مسیر `wp-content/themes/YourThemeName/` بروید (به جای `YourThemeName` نام واقعی قالب فعال خود را قرار دهید).
2.  فایل `functions.php` را باز کنید.
3.  کد زیر را به انتهای فایل (قبل از تگ بستن `?>`، در صورت وجود) اضافه کنید:

    ```php
    /**
     * Shortcode to display custom product sorting buttons.
     * Use [custom_sorting_buttons] in your pages or theme files.
     */
    function custom_sorting_buttons_shortcode() {
        ob_start();
        ?>
        <div class="custom-sorting-buttons">
            <a href="?orderby=popularity" class="sorting-button">محبوب</a>
            <a href="?orderby=rating" class="sorting-button">امتیاز</a>
            <a href="?orderby=date" class="sorting-button">جدید</a>
            <a href="?orderby=price" class="sorting-button">قیمت &uarr;</a>
            <a href="?orderby=price-desc" class="sorting-button">قیمت &darr;</a>
        </div>
        <?php
        return ob_get_clean();
    }
    add_shortcode('custom_sorting_buttons', 'custom_sorting_buttons_shortcode');

    /**
     * Optionally add basic styling for the buttons.
     * You should ideally style these buttons in your theme's stylesheet.
     */
    function custom_sorting_buttons_styles() {
        if ( is_shop() || is_product_category() || is_product_tag() || is_product_taxonomy() ) {
            ?>
            <style type="text/css">
                .custom-sorting-buttons {
                    display: flex;
                    gap: 10px;
                    margin-bottom: 20px;
                    flex-wrap: wrap; /* اجازه می‌دهد دکمه‌ها در صفحه‌های کوچک‌تر به خط بعدی بروند */
                    justify-content: flex-start; /* شروع از سمت چپ */
                }
                .custom-sorting-buttons .sorting-button {
                    background-color: #f0f0f0;
                    border: 1px solid #ccc;
                    padding: 8px 15px;
                    text-decoration: none;
                    color: #333;
                    border-radius: 5px;
                    transition: all 0.2s ease-in-out;
                    white-space: nowrap; /* جلوگیری از شکستن متن درون دکمه */
                }
                .custom-sorting-buttons .sorting-button:hover {
                    background-color: #e0e0e0;
                    color: #000;
                }
                .custom-sorting-buttons .sorting-button.active {
                    background-color: #0073aa; /* آبی وردپرس */
                    color: #fff;
                    border-color: #0073aa;
                }
                /* تنظیمات ریسپانسیو پایه */
                @media (max-width: 600px) {
                    .custom-sorting-buttons {
                        justify-content: center; /* در صفحه‌های کوچک‌تر دکمه‌ها را وسط چین می‌کند */
                    }
                }
            </style>
            <script type="text/javascript">
                document.addEventListener('DOMContentLoaded', function() {
                    const urlParams = new URLSearchParams(window.location.search);
                    const orderBy = urlParams.get('orderby');
                    const buttons = document.querySelectorAll('.custom-sorting-buttons .sorting-button');
                    buttons.forEach(button => {
                        const buttonHref = button.href;
                        // Extract query string from the button's href
                        const queryString = buttonHref.includes('?') ? buttonHref.split('?')[1] : '';
                        const buttonUrlParams = new URLSearchParams(queryString);
                        const buttonOrderBy = buttonUrlParams.get('orderby');

                        if (orderBy === buttonOrderBy) {
                            button.classList.add('active');
                        }
                    });
                });
            </script>
            <?php
        }
    }
    add_action('wp_head', 'custom_sorting_buttons_styles');
    ```

## نحوه استفاده

پس از نصب، به سادگی شورت‌کد `[custom_sorting_buttons]` را در هر صفحه، پست یا ویجت متنی که می‌خواهید دکمه‌های مرتب‌سازی در آن ظاهر شوند، اضافه کنید. این معمولاً در بالای حلقه محصولات ووکامرس در صفحات فروشگاه یا آرشیو قرار می‌گیرد.

**مثال برای قرار دادن در فایل‌های قالب:**

```php
<?php echo do_shortcode('[custom_sorting_buttons]'); ?>
```
## سفارشی‌سازی
استایل‌دهی: کد ارائه شده شامل CSS داخلی (inline CSS) برای دکمه‌ها است. برای استایل‌دهی قوی‌تر و یکپارچه‌تر با قالب، اکیداً توصیه می‌شود CSS را از تابع custom_sorting_buttons_styles به فایل style.css قالب خود یا یک فایل CSS اختصاصی منتقل کنید. سپس می‌توانید کلاس‌های .custom-sorting-buttons و .sorting-button را مطابق با طراحی سایت خود تغییر دهید.

**گزینه‌های مرتب‌سازی**: می‌توانید تگ‌های <a> را در داخل تابع custom_sorting_buttons_shortcode تغییر دهید تا متن یا گزینه‌های مرتب‌سازی را اضافه/حذف کنید. اطمینان حاصل کنید که پارامتر orderby در ویژگی href با گزینه‌های معتبر مرتب‌سازی ووکامرس مطابقت دارد (مثلاً menu_order, popularity, rating, date, price, price-desc, title).

**برجسته کردن دکمه فعال**: جاوااسکریپت موجود به طور پویا کلاس active را به دکمه مرتب‌سازی فعال اضافه می‌کند. می‌توانید این کلاس .active را در CSS خود استایل‌دهی کنید تا دکمه فعال برجسته شود.

## مشارکت (Contributing)
مشارکت شما خوشایند است! اگر پیشنهاد یا بهبودهایی برای این کد دارید، می‌توانید یک "Pull Request" ایجاد کنید یا "Issue" جدیدی را گزارش دهید.

## مجوز (License)
این پروژه تحت مجوز GPL-2.0-or-later منتشر شده است.
