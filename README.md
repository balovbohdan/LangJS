# Lang
Class to make multilanguage of web-site as easy as possible.

# Examples
## 1. Simple example
You can use this language class directly (see code below). But it is better to make subclasses of this class for different logical blocks of the web-site (see next example).

```javascript
// Make instance.
var lang = Lang.getReadyInst("/lang/user-profile/", "uk");

// Get language data elements.
var userAchievements = lang.get("achievements");
var userArticles = lang.get("articles");

// Use language data elements.
var h1 = document.createElement("h1"), h2 = document.createElement("h2");
h1.textContent = userAchievements;
h2.textContent = userArticles;
document.body.appendChild(h1);
document.body.appendChild(h2);
```

## 2. Better usage.
It is better when you make subclasses of the main language class for the different logical blocks of your web-site. For instance: user profile, news, articles, admin page, etc.
```javascript
/**
 * Language class for user profile page.
 * Singletone.
 * @param {Object} [params] Parameters. (See superclass for details.)
 */
function UserProfileLang(params) {
    if (UserProfileLang.__inst instanceof UserProfileLang) return UserProfileLang.__inst;
    UserProfileLang.__inst = this;
    params = params || {};
    Lang.call(this, { root: "/lang/user-profile/", lang: params.lang });
}

UserProfileLang.prototype = Object.create(Lang.prototype);
UserProfileLang.prototype.constructor = UserProfileLang;
UserProfileLang.prototype.name = "UserProfileLang";

/**
 * Makes ready instance.
 * Waits while language data is autoloading.
 * @param {string} [lang] Language code.
 * @returns {Promise<Lang>} Ready language instance.
 * @throws {Error}
 */
UserProfileLang.getReadyInst = function (lang) {
    return Lang.getReadyInstByClass(UserProfileLang, lang);
};
```

Do note that "UserProfileLang" class is singletone; it means that it is possible to make only one instance of it (when you creating instance more than one time you get the same instance).
So now we can rewrite code from the first example. As you can see it looks pretty good and laconically.

```javascript
// Make instance.
var lang = UserProfileLang.getReadyInst("uk");
```

This version of making language instance much more safe. Using classes of the certain logical web-site blocks you can't make mistake in the paths to language data folders. (And you don't need type the same paths again and again!)