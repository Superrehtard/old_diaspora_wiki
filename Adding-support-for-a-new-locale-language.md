#Adding support for a new locale/language

## Example : Adding support for 'np' locale / 'Nepali' language
* Open config/languages.yml in a text editor with UTF-8 encoding enabled.
* Under the 'available' section add :  
   * np: 'नेपाली'
* Create a copy of config/locales/devise/devise.en.yml as config/locales/devise/devise.np.yml
* Edit config/locales/devise/devise.np.yml and make necessary translations
* Create a copy of config/locales/diaspora/en.yml as config/locales/diaspora/np.yml
* Edit config/locales/diaspora/np.yml and make necessary translations
 