# PHP 7 coding style guide.

This guide describes the preferred PHP coding style within core [PHP.Gt](https://github.com/phpgt) repositories and as a basis for other projects' style guides.

Individual developers and teams are likely to have their own strong views on certain coding styles, so this set of documents should be seen as **a guide, not a rulebook**. That being said, everything that is documented here is the preference of core developers, so please consider the points within before submitting pull requests.

The intent of this document is to help guide the style of code contributions to maintain a consistent style throughout repositories, and to define and justify the decisions made within; some preferences of this styleguide are substantially different and in some areas unorthodox. For a more mainstream and widely adopted styleguide, please see [PHP Framework Interop Group's PSR-2][psr2].

[psr2]: http://www.php-fig.org/psr/psr-2/

## Overview.

The following example code shows an overview of the style proposed in this guide.

```php
<?php
namespace MyWebsite\Page;

use DateTime;
use MyWebsite\Thing\Action;
use MyWebsite\Thing\Response;
use MyWebsite\OtherThing\Container;

class Index_PageLogic extends \Gt\Page\Logic {

private $matchArray;
private $filterDetail;

public function go() : void {
	$count = 0;
	$expiry = new DateTime("-5 minutes");
	$action = new Action();

	if($action->isActive()) {
		$count += $this->exampleMethod();
	}
	else {
// when the action is not active, we should call the set active method before
// refreshing the action.
		$action->setActive($this->anotherExampleMethod());

		if($action->needsRefresh()
		&& $action->expiry < $expiry) {
			$action->refresh();
		}
	}
}

private function exampleMethod() : int {
	$this->matchArray = [
		"first" => Action::FIRST_VALUE,
		"second" => Action::SECOND_VALUE,
	];

	return count($this->matchArray);
}

private function anotherExampleMethod() : ?Container {
	foreach($this->matchArray as $key => $value) {
		Response::add($key, $value);
	}

	return Response::currentContainer();
}

}#
```

Please see a simplified bulleted list below. Click the headings for more information, examples and justification behind the decisions.

### [General concepts](general).

+ The first line of all `.php` files should be exactly: `<?php`.
+ Never use the closing PHP tag (`?>`).
+ Never use PHP short tags.
+ All files must only use `UTF-8`, without `BOM`.
+ Always use UTC timezone to store date and time.
+ Every `.php` library file should contain one class exactly.
+ Every `.php` library file should be side effect free.
+ Singular words (e.g. item, stylesheet, user) should be used throughout files and code instead of plurals.
+ Never use global variables, except for superglobals.
+ Properties should always be declared.
+ Variables should always be declared at the top of the scope they are used in.

### [Indentation and whitespace](indentation-whitespace).

+ Use the tab character for indentation, set to 4 spaces wide.
+ Avoid "double-indenting" blocks of code where no indentation is necessary for readability.
+ Do not leave whitespace at the end of lines.
+ Be as strict as is sensible to enforce 80 character lines.

### [Brackets and braces](brackets-braces).

+ Opening braces should always be placed at the end of the line.
+ Closing braces should always be placed at the start of their own line.
+ One level of indentation should be applied within, and only within, the braces.
+ Conditionals within brackets should not gain extra indentation when breaking onto separate lines.
+ Never leave conditional statements unbraced or unindented.

### [Spaces](spaces).

+ A space should follow all keywords.
+ A space should surround binary and ternary, but not unary operators.

### [Directories, files and namespaces](directories-files-namespaces).

+ Directories and files must either be namespace-mapped or web-mapped.
+ Namespaces must map to file paths, as per PSR-4.
+ Namespace-mapped files and directories must use `UpperCamelCase`.
+ Namespaces and classes must use `UpperCamelCase`.
+ Web mapped files and directories must use `lowercase-hyphenated-words`.

### [Classes](classes).

+ The term "class" refers to all classes, interfaces, and traits.
+ A class should only ever be autoloaded - never use `require` or `include`.
+ Constants should only be declared on a class.
+ Constants should use `UPPERCASE_UNDERSCORED`.
+ Properties should use `$camelCase`.
+ Methods should use `camelCase()`.
+ All parameters should define their type where possible.
+ All methods should define their return type where possible.
+ Methods should avoid becoming longer than 20-50 lines.
+ Classes should have all or no static methods.

### [Databases](databases).

+ Tables should be organised into a Table Collection Hierarchy.
+ Tables should use `UpperCamelCase`.
+ Tables in the same collection should use an underscore to split the hierarchy.
+ Columns should use `lowercase_underscored`.
+ Primary keys should follow `id_TableName`.
+ Columns can have optional descriptions to uniquely identify their purpose.
+ Foreign key column names should match their referenced column.
+ Foreign key constraint names should reference both tables and columns uniquely.

### [Commenting](commenting).

+ For inline comments, double-slash comments (`//`) should be used.
+ Inline comments should have no indentation, starting from column 1.
+ Inline comments can span multiple lines, prefixed with the double-slash.
+ Block comments should only be used to provide class and method documentation in the form of doc blocks.
+ Comments, along with docblocks, should be sparse and punchy to avoid stale documentation.

### [Data structures](data-structures).

+ Associative arrays should only be used for simple key-value-pairs.
+ Move associative arrays to an object's getter/setter storage where possible.
+ Static classes should only be used when truly stateless.
+ Private methods should be preferred over anonymous functions where appropriate.
+ Anonymous functions should never exceed 5 lines.
