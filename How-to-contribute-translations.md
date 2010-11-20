# How to contribute translations

There are two ways you can contribute a translation: with a pull request or at [[99translations.com|http://99translations.com]].

## How to choose the correct language code

First look if you find a existing translation.

If you have to create a new one look [[here|https://github.com/svenfuchs/rails-i18n/tree/master/rails/locale/]] or in config/locales/rails-i18n/ which code is used there for your language.

If your language isn't available there choose the right code according to [[ISO 639-1|http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes]] (or [[ISO 639-2|http://en.wikipedia.org/wiki/List_of_ISO_639-2_codes]] if your language has no ISO 639-1 code) in lowercase letters. If you want to create a country-specific one, that is not the most spoken type of it, append - and your country code according to [[ISO 3166-1|http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes]]  in uppercase letters.

Examples:

* de for German
* de-AT for German specific to Austria

If you want to create formal or informal versions of your translation append -informal or -formal to the version that is less common for social networks.

Examples:

* fr for the French formal one and
* fr-informal for the informal one

## Pull request

First read and follow [[Contributing to Diaspora: Using git|Git-Workflow]].

### Files

* config/locales/devise/devise.< code >.yml
* config/locales/diaspora/< code >.yml
* Leave config/locales/rails-i18n/ untouched, if you want to change something there send a pull request [[here|https://github.com/svenfuchs/rails-i18n]].

If you want to create a new translation copy the en files, choose the correct language code (see above) and change every occurence of en with your code. Don't forget the root element in the files!

## 99translations.com

First go to  [[99translations.com|http://99translations.com]] and create/sign-in to your account.

Then go to [[diaspora's project site|http://99translations.com/public_projects/show/181]] and join the team.

After that choose a file you want to work on [[here|http://99translations.com/projects/181]].

Choose your language or click "Create Translation" to create a new one. In the "Locale" field enter your language code (don't use 99translations.com's select boxes. To choose the correct language code see above).

You then will be presented the following view:

[[http://mrzyx.de/transtut/transtut1.png|width=550px]]

* At the "New" tab you can translate untranslated strings
* At the "Modified" tab you can retranslate strings that got updated in the master translation (the English one).
* At the "All" or the "Up-to-date" tab you can optimize already translated strings

When you click on "Edit" you will see the following:

[[http://mrzyx.de/transtut/transtut2.png|width=550px]]

* You can see the original text from the master file (the English translation file).
* You can enter the translation in the textbox
* And then click "Save and continue" to get to the next translation (Or skip this one with "Next").

### Note

Please do not try to upload translations to 99translations.com. There are some weird bugs with that. You can instead send them to diaspora@mrzyx.de and I'll see how I can add them.