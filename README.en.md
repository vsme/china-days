# China Days

![NPM Version](https://img.shields.io/npm/v/china-days)
![GitHub License](https://img.shields.io/github/license/vsme/china-days)
[![README](https://img.shields.io/badge/README-中文-brightgreen.svg)](https://github.com/vsme/china-days/blob/main/README.md)

> Translated by ChatGPT-4, PRs are welcome.

This project provides a set of functions for managing and querying Chinese holidays, adjusted workdays (in lieu days), regular workdays, and the 24 solar terms. By using these functions, users can easily check the status of a specified date, get holidays or workdays within a date range, and find specific workdays. Additionally, the project supports querying the dates of the 24 solar terms, helping users understand the timing of traditional Chinese solar terms.

Supports the years 2004 to 2024, including the extended Spring Festival in 2020.

## Quick Start

Include directly in your browser:

```html
<script src="https://cdn.jsdelivr.net/npm/china-days/dist/index.min.js"></script>
```

Installation:

```sh
npm i china-days
```

Using ESM import:

```ts
import chinaDays from 'china-days'
console.log(chinaDays)
```

Using in Node.js:

```js
const { isWorkday, isHoliday } = require('china-days');
console.log(isWorkday('2020-01-01'));
console.log(isHoliday('2020-01-01'));
```

## Holiday Module

### `isWorkday` Check if a date is a workday

```js
console.log(isWorkday('2023-01-01')); // false
```

### `isHoliday` Check if a date is a holiday

```js
console.log(isHoliday('2023-01-01')); // true
```

### `isInLieu` Check if a date is an in lieu day

In China's holiday arrangement, an in lieu day is a workday or a rest day adjusted for consecutive holidays or make-up workdays. For example, if a public holiday is connected to a weekend, a weekend day might be adjusted to a workday, or a workday might be adjusted to a rest day for a longer consecutive holiday.

```js
// Check if 2024-05-02 is an in lieu day. Returns `true` if it is.
console.log(isInLieu('2024-05-02')); // true

// Check if 2024-05-01 is an in lieu day. Returns `false` if it is not.
console.log(isInLieu('2024-05-01')); // false
```

### `getDayDetail` Check if a specified date is a workday

This function checks if a specified date is a workday and returns a boolean indicating if it's a workday and details about the date.

1. If the specified date is a workday, it returns true and the name of the workday. If it's a make-up workday, it returns true and holiday details.
2. If it's a holiday, it returns false and holiday details.

```js
// Example usage

// Regular workday, Friday
console.log(getDayDetail('2024-02-02')); // { "date": "2024-02-02", "work":true,"name":"Friday"}
// Holiday, Saturday
console.log(getDayDetail('2024-02-03')); // { "date": "2024-02-03", "work":false,"name":"Saturday"}
// Make-up workday
console.log(getDayDetail('2024-02-04')); // { "date": "2024-02-04", "work":true,"name":"Spring Festival,春节,3"}
// Holiday, Spring Festival
console.log(getDayDetail('2024-02-17')); // { "date": "2024-02-17", "work":false,"name":"Spring Festival,春节,3"}
```

### `getHolidays` Get all holidays within a specified date range

Receives start and end dates and optionally includes weekends. If weekends are included, the function returns all holidays including weekends; otherwise, only holidays on weekdays are returned.

> Tip: Even if weekends are not included, holidays that fall on weekends will still be returned.

```js
// Example usage
const start = '2024-04-26';
const end = '2024-05-06';

// Get all holidays from 2024-05-01 to 2024-05-10, including weekends
const holidaysIncludingWeekends = getHolidays(start, end, true);
console.log('Holidays including weekends:', holidaysIncludingWeekends.map(d => getDayDetail(d)));

// Get holidays from 2024-05-01 to 2024-05-10, excluding weekends
const holidaysExcludingWeekends = getHolidays(start, end, false);
console.log('Holidays excluding weekends:', holidaysExcludingWeekends.map(d => getDayDetail(d)));
```

### `getWorkdays` Get a list of workdays within a specified date range

Receives start and end dates and optionally includes weekends. If weekends are included, the function returns all workdays including weekends; otherwise, only weekdays (Monday to Friday) workdays are returned.

```js
// Example usage
const start = '2024-04-26';
const end = '2024-05-06';

// Get all workdays from 2024-05-01 to 2024-05-10, including weekends
const workdaysIncludingWeekends = getWorkdays(start, end, true);
console.log('Workdays including weekends:', workdaysIncludingWeekends);

// Get workdays from 2024-05-01 to 2024-05-10, excluding weekends
const workdaysExcludingWeekends = getWorkdays(start, end, false);
console.log('Workdays excluding weekends:', workdaysExcludingWeekends);
```

### `findWorkday` Find a workday

Find the workday that is {deltaDays} workdays from today.

```js
// Find the workday that is {deltaDays} workdays from today
// If deltaDays is 0, first check if today is a workday. If it is, return today.
// If today is not a workday, find the next workday.
const currentWorkday = findWorkday(0);
console.log(currentWorkday);

// Find the next workday from today
const nextWorkday = findWorkday(1);
console.log(nextWorkday);

// Find the previous workday from today
const previousWorkday = findWorkday(-1);
console.log(previousWorkday);

// You can pass a second parameter to find a specific date's workdays
// Find the second workday from 2024-05-18
const secondNextWorkday = findWorkday(2, '2024-05-18');
console.log(secondNextWorkday);
```

## Solar Terms Module

### Get dates of the 24 solar terms

```js
import { getSolarTerms } from "china-days";

/** Get an array of solar term dates within a range */
const solarTerms = getSolarTerms("2024-05-01", "2024-05-20");
solarTerms.forEach(({ date, term, name }) => {
  console.log(`${name}: ${date}, ${term}`);
});
// 立夏: 2024-05-05, the_beginning_of_summer
// 小满: 2024-05-20, lesser_fullness_of_grain

// No solar terms, return []
getSolarTerms("2024-05-21", "2024-05-25");
// return []

/* If end parameter is not provided, get the solar term for a specific day */
getSolarTerms("2024-05-20");
// return: [{date: '2024-05-20', term: 'lesser_fullness_of_grain', name: '小满'}]
```

## Contributing

1. Fork + Clone the project locally;
2. Modify [holiday definitions](scripts/generate.ts);
3. Run `npm run generate` to automatically generate [constants file](src/holidays/constants.ts);
4. Submit a PR.

## Acknowledgments

This project references the `Python` version of the [LKI/chinese-calendar](https://github.com/LKI/chinese-calendar) open-source project.