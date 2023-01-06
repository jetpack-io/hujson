# Jetpack.io's Human JSON

The `go.jetpack.io/hujson` package implements an extension
of [standard JSON](https://datatracker.ietf.org/doc/html/rfc8259) that is easier
for humans to write and modify. It's based on Tailscale's corresponding [HuJSON library](https://github.com/tailscale/hujson) but allows for some additional syntax.

Example:
```
{
    /* Block comments and multiline strings */
    multi: `
      This is a
      multiline string
    `,

    bar: "baz", // Line comments are allowed and unquoted keys
    foo: "bar", // Trailing commas are allowed too
}
```

Jetpack.io's HuJSON format permits the following additions over standard JSON:

## 1. Comments
C-style line comments and block comments can be intermixed with whitespace.

Example:
```json
{
    // This is a line comment
    "foo": "bar", /* This is a block comment */
    "baz": "quux"
}
```

## 2. Trailing Commas
Trailing commas after the last member/element in an object/array are allowed.

Examples:
```json
{
    "foo": "bar", // It's ok to have a comma in the last element
}
```

As a result, you can comment out the last entry in an object or array, and the commas still parse correctly:

```json
{
    "foo": "bar",
    // "baz": "quux"
}
```

## 3. Multiline strings
Multiline strings are allowed. Multiline strings use JavaScript's [template strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) syntax, and are delimited with a backtick (``` ` ```).

They are particularly useful for writing long strings that contain newlines.

Example:
```json
{
    "foo": `This is a multiline string
    that contains a newline`,

    // This is an inline bash script:
    "script": `
        #!/bin/bash
        echo "Hello, world!"
    `
}
```

## 4. Unquoted keys
The key for an object can be written without quotes if it starts with a letter
(a-z, A-Z) and is followed by letters, numeric digits, or underscores.
Javscript, and Typescript already allow for unquoted keys (but standard JSON does not)

Example:
```json
{
    foo: "bar",
    baz_quux: "quuz"
}
```

