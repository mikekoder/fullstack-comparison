# Translations
## Angular
``` html
<!-- simple -->
<span i18n="@@simple">Default text</h1>
<!-- interpolation -->
<span i18n="@@interpolated">Text {{name}}</span>
<!-- plural -->
<span i18n>{count, plural, =0 {no items} =1 {one item} other {{{count}} items}}</span>
```

Or in the code
``` ts
const simple = $localize`:@@simple_key:Default text`;
```

## Blazor
``` html
@inject Microsoft.Extensions.Localization.IStringLocalizer<MyComponent> Loc
<!-- simple -->
<span>@Loc["Simple"]</span>
<!-- formatted -->
<span>@String.Format(Loc["Interpolation with first name {0} and last name{1}"].Value, Model.FirstName, Model.LastName)</span>
<!-- plural -->
<span>@(count == 1 ? Loc["Singular"] : Loc["Plural"])</span>
```

## React
https://react.i18next.com/

``` json
{
  "simple": "text",
  "withNamedParameter": "text {{name}}",
  "simplePluralized": "one item",
  "simplePluralized_plural": "{{count}} items"
}
```

``` jsx
<div>{t('simple')}</div>

<Trans i18nKey="withNamedParameter">
  text {{ name: person.name }}
</Trans>

<Trans i18nKey="simplePluralized" count={messages.length}>
  You got {{ count: messages.length }}.
</Trans>
```
## Svelte (svelte-i18n)
``` ts
const messages = {
  simple: 'text',
  withNamedParameter: 'text {name}',
  pluralizedWithNumber: ' {n, plural, =0 {no items} one { one item } other {# items }',
  withDate: "text {date}"
}
```

```ts
$_('simple')
$_('withNamedParameter', { values: { name: 'value' }})
$_('pluralizedWithNumber', { n: nominationCount })
$_('withDate', { date: $_.date(new Date(), 'long')})
```

## Vue (vue-i18n)
``` ts
const messages = {
  simple: 'text',
  withNamedParameter: 'text {name}',
  withIndexedParameter: 'text {0}',
  simplePluralized: 'one item | many items',
  pluralizedWithNumber: 'no items | one item | {count} items',
}
```
``` ts
$t('simple')
$t('withNamedParameter', { name: 'value' })
$t('withIndexedParameter', ['value'])
$tc('simplePluralized', 1)
$tc('pluralizedWithNumber', 5, { count: 5 })
{{ $d(new Date(), 'long', 'fi-FI') }}
{{ $n(100, 'currency', 'fi-FI') }}
```
