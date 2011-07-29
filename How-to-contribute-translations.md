# How to contribute translations

There are two ways you can contribute a translation: with a pull request or at [[99translations.com|http://99translations.com]].
But lets start with some....

## General Notes

### Languages with high inflection and grammar different to English

We integrated the awesome extensions @siefca made for the [i18n Gem](https://github.com/svenfuchs/i18n) and for Rails: [i18n-inflector](https://github.com/siefca/i18n-inflector) and [i18n-inflector-rails](https://github.com/siefca/i18n-inflector-rails). Based on what the user has written into the gender field and our [definitions](https://github.com/diaspora/diaspora/tree/master/config/locales/inflections) the gender is guessed. Look at the readme of i18n-inflector for more documentation and how to use them. You can use the "named patterns" easily via 99translations if you want to do so, however if you want to contribute a definition please make a [pull request](https://github.com/diaspora/diaspora/wiki/Git-Workflow).

### Choose the correct language code

First look if you find an existing translation.

If you have to create a new one look [[here|https://github.com/svenfuchs/rails-i18n/tree/master/rails/locale/]] or in config/locales/rails-i18n/ which code is used there for your language.

If your language isn't available there choose the right code according to [[ISO 639-1|http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes]] (or [[ISO 639-2|http://en.wikipedia.org/wiki/List_of_ISO_639-2_codes]]/3 if your language has no ISO 639-1 code) in lowercase letters. If you want to create a country-specific one, that is not the most spoken type of it, append - and your country code according to [[ISO 3166-1|http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes]]  in uppercase letters.

Examples:

* de for German
* de-AT for German specific to Austria

If you want to create formal or informal versions of your translation append \_informal or \_formal to the version that is less common for social networks.

Examples:

* fr for the French formal one and
* fr_informal for the informal one

## Pull request
**Note:** If you want to take the responsibility for you language this way is fine, however using both methods at a time is too much work, so please provide constant updates if you want to do it this way. Keep in mind that you have to watch for changes yourself, there's no automatism that updates anything with this way. Also get sure to be the person to contact about it in your local community and add a way to contact you/contribute to the translation to the *Languages handled outside of 99translations* section below.

First read and follow [[Contributing to Diaspora: Using git|Git-Workflow]].

### Files

* config/locales/devise/devise.< code >.yml
* config/locales/diaspora/< code >.yml
* config/locales/javascript/javascript.< code >.yml
* Leave config/locales/rails-i18n/ untouched, if you want to change something there send a pull request [[here|https://github.com/svenfuchs/rails-i18n]].
* Only add something to config/locales/inflections/< code >.yml if you know what you're doing, look at the note about inflected languages above for more informations. If you're unsure just ignore that directory.
* config/locales/cldr/ is too an upstream resource. You shouldn't need to touch it.

If you want to create a new translation copy the en files, choose the correct language code (see above) and change every occurrence of en with your code. Don't forget the root element in the files!

If you want to fix an untranslatable strings or something feel free, but only add the new key to en.yml/devise.en.yml/javascript.en.yml.

### "Permanent Pull Requst"

If you want to give constant updates you can also notify me (@MrZYX, IRC: MrZYX, E-Mail: diaspora@mrzyx.de) and give me the URL to a Git Repository and I'll do regular pulls in my update workflow.

## 99translations.com

### Languages handled outside of 99translations

* [[ca|https://gitorious.org/diaspora-l10n-ca/diaspora-l10n-ca]]
* [[gl|http://trasno.net/]]
* [[ml (Malayalam)|http://wiki.smc.org.in/Diaspora_Localization]]

### Uploading locales to 99translations
Please do not try to upload translations to 99translations.com after you've downloaded them to edit them in a texteditor.  The reasons are:

* If you made syntax errors that made the YAML file invalid, 99translations is terrible at informing you about that
* If someone else made changes while you're editing the file, they would be overwritten by yours.
* If you leave English strings in the file, 99transaltions would treat them as complete translations.

You can instead send them to diaspora@mrzyx.de and I'll see how I can add them. But if you do so please try to provide as complete translations as possible, I don't have the time to do every minor upload. If you want to provide constant updates, you can choose one of the methods above or upload them by yourself, but get really sure to avoid the mentioned issues.

### Translating with 99translations

First go to  [[99translations.com|http://99translations.com]] and create/sign-in to your account.

Then go to [[diaspora's project site|http://99translations.com/public_projects/show/181]] and join the team.

After that choose a file you want to work on [[here|http://99translations.com/projects/181]].



Choose your language or click "Create Translation" to create a new one. In the "Locale" field enter your language code (**don't use 99translations.com's select boxes. To choose the correct language code see above.**).

You then will be presented the following view:

[[https://mrzyx.de/transtut/transtut1.png|width=550px]]

* At the "New" tab you can translate untranslated strings
* At the "Modified" tab you can retranslate strings that got updated in the master translation (the English one).
* At the "All" or the "Up-to-date" tab you can optimize already translated strings

When you click on "Edit" you will see the following:

[[https://mrzyx.de/transtut/transtut2.png|width=550px]]

* You can see the original text from the master file (the English translation file).
* You can enter the translation in the textbox
* **Note:** have a look at the key (under "Edit Translation"). It can help you with the context, especially for gender specific translations.
* And then click "Save and continue" to get to the next translation (Or skip this one with "Next").