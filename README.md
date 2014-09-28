CCS
===

Cascading Coffee Scripts is a simple CoffeeScript class that constructs Cascading Style
Sheets from functions. The project is **not** currently ready for use.

To use CCS, you pass the constructor a function which defines the styles.

    styles = new CCS ->

        fonts =
            mono: "Ubuntu Mono"
            sans: "Comic Sans"

        colors =
            red: "#FF0000"
            background: "#EEEEFF"

        @selector ".foo", ->
            @font fonts.mono
            @bgColor colors.background

            @selector ".bar", ->
                @font fonts.sans
                @size 12

        @selector ".spam", -> @color colors.red

If you ran that code, then `styles.tree` would evaluate to:

    {
        ".foo": {
            "font-family": "Ubuntu Mono",
            "background-color": "#EEEEFF",
            ".bar": {
                "font-family": "Comic Sans",
                "font-size": "12px"
            }
        },
        ".spam": {
            "color": "#FF0000"
        }
    }

## Limitations

The property functions, like `@font`, need implementing. Only examples currently exist.
The function that converts the tree to a string has yet to be pushed too.

It's a work in progress.
