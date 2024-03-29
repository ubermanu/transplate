# transplate

This package provides a simple translation system for JavaScript applications.

## Features

- A single function (template literal) to translate strings
- A lexicon that defaults to English
- Supports placeholder formats
- Supports pluralization, according to the CLDR rules
- Supports contextualization

## Installation

```bash
npm install transplate
```

## Usage

```javascript
import { Dictionary, registerDictionary, t } from 'transplate'

const dict = new Dictionary('jp')

dict.addTranslation({
  id: 'Hello, %s!',
  strings: ['こんにちは、%s！'],
})

registerDictionary(dict)

const username = 'John'
t`Hello, ${username}!` // Hello, John！

setLocale('jp')
t`Hello, ${username}!` // こんにちは、John！
```

## API

### t

Translates a string in the defined locale, using a template literal.

```json
{
  "id": "Hello!",
  "strings": ["Salut!"]
}
```

```javascript
import { t } from 'transplate'

t`Hello!` // Salut!
```

Supports placeholders, if defined in the translation:

```json
{
  "id": "Hello, %s!",
  "strings": ["Salut, %s!"]
}
```

```javascript
import { t } from 'transplate'

const name = 'John'
t`Hello, ${name}!` // Salut, John!
```

Supports pluralization:

```json
{
  "id": "You have %d new message.",
  "plural_id": "You have %d new messages.",
  "strings": [
    "Vous avez %d nouveau message.",
    "Vous avez %d nouveaux messages."
  ]
}
```

```javascript
import { t } from 'transplate'

t`You have ${1} new message.` // Vous avez 1 nouveau message.
t`You have ${877} new message.` // Vous avez 877 nouveaux messages.
```

Supports contextualization:

```json
{
  "id": "Welcome Mr %s",
  "strings": ["Bienvenue M. %s"],
  "context": "m"
}
```

```json
{
  "id": "Welcome Mr %s",
  "strings": ["Bienvenue Mme %s"],
  "context": "f"
}
```

```javascript
import { t } from 'transplate'

const data = {
  firstname: 'Sarah',
  gender: 'f',
}

// Generates a new translation function with the given context
const tx = t.context(data.gender)

tx`Welcome Mr ${data.firstname}` // Bienvenue Mme Sarah
```

### getLocale

Returns the current locale. Defaults to `en`.

```javascript
import { getLocale } from 'transplate'

getLocale() // en
```

### setLocale

Sets the current locale in the ISO 639-1 format.

```javascript
import { setLocale } from 'transplate'

setLocale('fr')
```

### detectLocale

Detects the current locale, using the current process' environment variables or the browser's `navigator.language`.

```javascript
import { detectLocale } from 'transplate'

detectLocale() // the locale has been set to `xx`
```

### registerDictionary

Registers a dictionary for a given language, in the global lexicon.

```javascript
import { registerDictionary, Dictionary } from 'transplate'

const dict = new Dictionary('fr')

dict.addTranslation({
  id: 'Hello!',
  strings: ['Salut!'],
})

registerDictionary(dict)
```
