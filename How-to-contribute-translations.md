# How to contribute translations

There are two ways you can contribute a translation: with a pull request or at [[Web translate it|https://webtranslateit.com]].
But lets start with some....

## General Notes

### Languages with high inflection and grammar different to English

We integrated the awesome extensions @siefca made for the [i18n Gem](https://github.com/svenfuchs/i18n) and for Rails: [i18n-inflector](https://github.com/siefca/i18n-inflector) and [i18n-inflector-rails](https://github.com/siefca/i18n-inflector-rails). Based on what the user has written into the gender field and our [definitions](https://github.com/diaspora/diaspora/tree/master/config/locales/inflections) the gender is guessed. Look at the readme of i18n-inflector for more documentation and how to use them. You can use the "named patterns" easily via Web translate it if you want to do so, however if you want to contribute a definition please make a [pull request](https://github.com/diaspora/diaspora/wiki/Git-Workflow).

### Choose the correct language code

First look if you find an existing translation.

If you have to create a new one look [[here|https://github.com/svenfuchs/rails-i18n/tree/master/rails/locale/]] which code is used there for your language.

If your language isn't available there choose the right code according to [[ISO 639-1|http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes]] (or [[ISO 639-2|http://en.wikipedia.org/wiki/List_of_ISO_639-2_codes]]/3 if your language has no ISO 639-1 code) in lowercase letters. If you want to create a country-specific one, that is not the most spoken type of it, append - and your country code according to [[ISO 3166-1|http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes]]  in uppercase letters.

Examples:

* de for German
* de-AT for German specific to Austria

If you want to create formal or informal versions of your translation append \_informal or \_formal to the version that is less common for social networks.

Examples:

* fr for the French formal one and
* fr_informal for the informal one

## Pull request
**Note:** If you want to take the responsibility for you language this way is fine, however using both methods at a time is too much work, so please provide constant updates if you want to do it this way. Keep in mind that you have to watch for changes yourself, there's no automatism that updates anything with this way. Also get sure to be the person to contact about it in your local community and add a way to contact you/contribute to the translation to the *Languages handled outside of Web translate it* section below.

First read and follow [[Contributing to Diaspora: Using git|Git-Workflow]].

### Files

* config/locales/devise/devise.< code >.yml
* config/locales/diaspora/< code >.yml
* config/locales/javascript/javascript.< code >.yml
* Only add something to config/locales/inflections/< code >.yml if you know what you're doing, look at the note about inflected languages above for more informations. If you're unsure just ignore that directory.
* config/locales/cldr/ is an upstream resource too. You shouldn't need to touch it.

If you want to create a new translation copy the en files, choose the correct language code (see above) and change every occurrence of en with your code. Don't forget the root element in the files!

If you want to fix an untranslatable strings or something feel free, but only add the new key to en.yml/devise.en.yml/javascript.en.yml.

### "Permanent Pull Requst"

If you want to give constant updates you can also notify me (@MrZYX, IRC: MrZYX, E-Mail: diaspora@mrzyx.de) and give me the URL to a Git Repository and I'll do regular pulls in my update workflow.

## Web translate it

### Languages handled outside of Web translate it

* [[ca|https://gitorious.org/diaspora-l10n-ca/diaspora-l10n-ca]]
* [[gl|http://trasno.net/]]

### Translating with Web translate it

First go to  [[Web translate it|https://webtranslateit.com/en/sign_up]] and create/sign-in to your account.

Then go to [[diaspora's project site|https://webtranslateit.com/en/projects/3020-Diaspora]] and join the team.

[[https://mrzyx.de/transtut/wti_1.png|width=550px]]

Choose your language or suggest a new one and request an invitation.

[[https://mrzyx.de/transtut/wti_2.png|width=550px]]

You'lll receive an email containing a link to accept your invitation. Note that this can take one or two days since I have to manually approve your request ;)
Once you got the mail and clicked on the link you'll see something like this:

[[https://mrzyx.de/transtut/wti_3.png|width=550px]]

After you accepted it you can click on the "Translations" tab and start translating:

[[https://mrzyx.de/transtut/wti_4.png|width=550px]]

Thank you for contributing!
