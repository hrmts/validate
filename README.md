# @Hrmts/Validate
[![NPM version][npm-image]][npm-url] [![Downloads][downloads-image]][npm-url]

##Info
**Error messages may be subject to change** 

This is a validation library that not only returns true or false depending on validation result, but it also returns a spesific error message.

This project is inpired by [https://npmjs.org/package/validator](https://npmjs.org/package/validator) . 

## Installation 

	npm install @hrmts/validate
	
###usage 
	import validate from '@hrmts/validate'
	console.log(validate.textInput("Yes")) // { isValid: true, message: "" }
You can also import a single validator if you like. 
	
	import {textInput} from '@hrmts/validate/lib/textInput'
	console.log(textInput("Yes")); // { isValid: true, message: "" }


###Validators 

- **checkDay(day, month, year, startDay = 1, endDay = 31)** - check if a day is valid. This takes into account for leapYears. 
if startDay is greater than endDay, it will throw an error.
```javascript
validate.checkDay(29, 2, 2016); // { isValid: true, message: "" }
validate.checkDay(29, 2, 2015); // { isValid: false, message: "Day can't be greater than 28" }
validate.checkDay(31, 7, 2015); // { isValid: true, message: "" }
validate.checkDay(31, 6, 2015); // { isValid: false, message: "Day can't be greater than 30" }
validate.checkDay(28, 6, 2015, undefined, 20); // { isValid: false, message: "Day can't be greater than 20" }
validate.checkDay(10, 6, 2015, 15); // { isValid: false, message: "Day can't be less than 15" }
validate.checkDay("1w", 2, 2016); // { isValid: false, message: "Day must be an Integer" }
```

- **checkMonth(month, startMonth = 1, endMonth = 12)** - checks if month is valid.
if startMonth is greater than endMonth, it will throw an error.
```javascript
validate.checkMonth(2); // { isValid: true, message: ""}
validate.checkMonth(2, 3); { isValid: false, message: "Month can't be less than 3" } 
validate.checkMonth("2s", 3); { isValid: false, message: "Month must be a Integer" } 
```

- **checkYear(year, startYear = 0 , endYear = new Date().getFullYear())** - checks if year is beetwen startyear and endYear. If startYear is greater than endYear, it will throw an error.

```javascript
validate.checkYear(2016); // { isValid: true, message: ""}
validate.checkYear(2000, 2005); // { isValid: false, message: "Year can't be less than 2005" }
validate.checkYear(2016, undefined, 2015); // { isValid: false, message: "Year can't be greater than 2015" }
validate.checkYear("s2", undefined, 2015); // { isValid: false, message: "Year must be a Integer" }
```

- **isAfter(toDate, fromDate = { day: 1, month: 1, year: 0 }, endDate=todaysDate )** - lets say you have two dates, a from and a to date. this is validation that to date is not less than from date.
This function just calls `isDate(toDate, fromDate, toDate);`.

- **isBefore(fromDate, toDate = todaysDate, startDate = { day: 1, month: 1, year: 0 } )** - lets say you have two dates, a from and a to date. this is validation that from date is not greater than to date.
This function just calls `isDate(toDate, fromDate, endDate);`.

- **isDate(date, startDate = { day: 1, month: 1, year: 0 }, endDate = todaysDate)** - checks if a date is valid, checks also if date is beetween startDate and endDate.
```javascript
validate.isDate({ day: 20, month: 04, year: 1991 }); // { isValid: true, message: "" }
validate.isDate({ day: 20, month: 04, year: 1991 }, { day: 21, month: 04, year: 1991 }); // { isValid: false, message: "Day can't be less than 21" }
validate.isDate({ day: 20, month: 04, year: 1991 }, undefined, { day: 19, month: 04, year: 1991 }); // { isValid: false, message: "Day can't be greater than 19" }
```

- **isGreater(value, MAX_LENGTH=150, name="input")** - checks if the string is less than minimum requirement.
```javascript
validate.isGreater(""); // { isValid: true, message: "" }
validate.isGreater("foo", 1); // { isValid: false, message: "input cannot be greater than 1 chars" }
validate.isGreater("foo",2, "bar"); // { isValid: false, message: "bar cannot be greater than 2 chars" }
validate.isGreater("foo", undefined, "bar"); // { isValid: true, message: "" }
```

- **isLesser(value, MIN_LENGTH = 0, name = "input")** - checks if the string is less than minimum requirement.
```javascript
validate.isLesser(""); // { isValid: true, message: "" }
validate.isLesser("", 1); // { isValid: false, message: "input cannot be empty" }
validate.isLesser("s",2, "bar"); // { isValid: false, message: "bar cannot be less than 2 chars" }
validate.isLesser("foo", 2); // { isValid: true, message: "" }
```

- **isInt(value)** - this is the same method as Number.isInteger(value). 
```javascript
validate.isInt(2); //true
validate.isInt(2.1); //false
validate.isInt("2kl"); // false
```

- **isLeapYear(year)** - checks if the year is a leap year
```javascript
validate.isLeapYear(1900); // false
validate.isLeapYear(2016); // true
validate.isLeapYear(2017); // false
```

- **isNull(value, name = "input")**
```javascript
validate.isNull(null); // { isValid:false, message: "input cannot be empty" }
validate.isNull(null, "bar"); // { isValid:false, message: "bar cannot be empty" }
validate.isNull("foo", "bar"); // { isValid:true, message: "" }
```

- **isUndefined(value, name="input")** - checks if input is undefined. 
```javascript
validate.isUndefined(undefined); // { isValid:false, message: "input cannot be empty" }
validate.isUndefined(undefined, "bar"); // { isValid:false, message: "bar cannot be empty" }
validate.isUndefined("foo", "bar"); // { isValid:true, message: "" }
```

- **textInput(value, name ="input", min=0, max=150)** - validates textInput with these rules: isNull, isUndefined, isLesser, isGreater.

- **customTextInput(validationRules)** - customize your own textInput validation. validationRules must be an array containing validation functions.
```javascript 
var validationRules = [];
validationRules.push(validate.isNull(value, name));
validationRules.push(validate.isUndefined(value, name));
validationRules.push(validate.isLesser(value, min, name));
validationRules.push(validate.isGreater(value, max, name));
```
    

[downloads-image]: https://img.shields.io/npm/dt/@hrmts/validate.svg

[npm-url]: https://npmjs.org/package/@hrmts/validate
[npm-image]: http://img.shields.io/npm/v/@hrmts/validate.svg
