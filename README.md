# Checklist Before Submitting WP Plugin
Comprehensive checklist to make sure before submitting WordPress plugin, these were the issues I had experienced while submitting my earlier plugins, I have shared following checklist incase you have plan to submitt your WordPress plugins to publish them on WordPress.org plugin directory. I will keep sharing more points if I will be having in future from WordPress. Please feel free to fork and add your checklist points.

- [x] **Plugin Name**

Plugin name should not be like to include the word ‘plugin’ or ‘wordpress’ in them (it’s redundant after all), but also becuase names that are too long, or too short, or too generic are really annoying.

- [x] **Generic function (and/or define) names**

All plugins must have unique function names, defines, and classnames. This prevents your plugin from conflicting with other plugins or themes.

For example, if your plugin is called "Easy Custom Post Types", then you might prefix your functions with ecpt_{your function name here}. Similarly a define of LICENSE would be better done as ECPT_LICENSE. You can use namespaces instead, however make sure that those also are unique. A namespace or class of 'MyPlugin' is NOT actually all that unique.

This extends to anything in a define. For example, if you were to use this, it would be a bad idea:

```define( 'PLUGIN_PATH', plugins_url( __FILE__ ) );```

That define is a global, so PLUGIN_PATH could conflict with a number of other things.

Don't try to use two letter slugs anymore. As of 2016, all the good ones are taken. Instead consider easy_cpts_ (from the first example).

Similarly, don't use __ (double underscores), wp_ , or _ (single underscore) as a prefix. Those are reserved for WordPress itself. You can use them inside your classes, but not as stand-alone function.


- [x] **Don't mess with error reporting**

While error_reporting() is a great tool in PHP (http://www.php.net/manual/en/function.error-reporting.php ) but if you set it permanently in your plugin, you mess things up for everyone who uses your code. Should they have a reason to try to debug their site which happens to use your code, they won't be able to get a clean test because you're messing with the output. It has no place in the day to day function of your plugin.

do not use any code like this ```error_reporting(E_ALL);```

- [x] **Don't set a default timezone**

This is rarely a good idea. People should be able to define their own timezones in WordPress.

- [x] **Calling file locations poorly**

When you hardcode in paths, or assume that everyone has WordPress in the root of their domain, you cause anyone using 'Giving WordPress it's own directory' (a VERY common setup) to break. In addition, WordPress allows users to change the name of wp-content, so you would break anyone who choses to do so.

Please review http://codex.wordpress.org/Determining_Plugin_and_Content_Directories and update your plugin accordingly. And don't worry about supporting WordPress 2.x or lower. We don't encourage it nor expect you to do so, so save yourself some time and energy.

Example: ```site_url() . '/wp-content/plugins/plugin-slug/style.css' );```

You should use this: ```plugins_url( 'style.css' , __FILE__ );```

- [x] **Please sanitize, escape, and validate your POST calls**

When you include POST/GET/REQUEST calls in your plugin, it's important to sanitize, validate, and escape them. The goal here is to prevent a user from accidentally sending trash data through the system, as well as protecting them from potential security issues.

SANITIZE: All instances where generated content is inserted into the database, or into a file, or being otherwise processed by WordPress, the data MUST be properly sanitized for security. By sanitizing your POST data when used to make action calls or URL redirects, you will lessen the possibility of XSS vulnerabilities. You should never have a raw data inserted into the database, even by a update function, and even with a prepare() call.

VALIDATE: In addition to sanitization, you should validate all your calls. If a $_POST call should only be a number, ensure it's an int() before you pass it through anything. Even if you're sanitizing or using WordPress functions to ensure things are safe, we ask you please validate for sanity's sake. Any time you are adding data to the database, it should be the right data.

ESCAPE: Similarly, when you're outputting data, make sure to escape it properly, so it can't hijack admin screens. There are many esc_*() functions you can use to make sure you don't show people the wrong data.

In all cases, using stripslashes or strip_tags is not enough. You need to use the most appropriate method associated with the type of content you're processing. Check that a URL is a URL and don't just be lazy and use sanitize_text please. The ultimate goal is that you should ensure that invalid and unsafe data is NEVER processed or displayed. Clean everything, check everything, escape everything, and never trust the users to always have input sane data.

- [x] **Not using Nonces and/or checking permissions**

Please add a nonce to your POST calls to prevent unauthorized access.

Keep in mind, check_admin_referer alone is NOT bulletproof security. Do not rely on nonces for authorization purposes. Use ```current_user_can()``` in order to prevent users without the right permissions from accessing things.

https://codex.wordpress.org/WordPress_Nonces



