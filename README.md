# Cascading Coffee Scripts

CCS is a simple CoffeeScript class that constructs Cascading Style
Sheets from functions. The project is **not** currently ready for use.

To use CCS, you pass the constructor a function which defines the styles.

    styles = new CCS ->

        # This is a user defined enclosure. @ is always the new
        # CCS instance.

        red = "#FF0000"
        bgColor = "#EEEEFF"
        fonts = mono: "Ubuntu Mono", sans: "Comic Sans"

        # You can define brushes using @brush as a setter.

        @brush "sansPink", ->
            @font "sans"
            @color "pink"

        # You can extend and overload the methods of the new CCS instance.

        @foo = (bottom) ->
            @font fonts.sans
            @setPosition "fixed", 0, 0, bottom, 25

        # Once the scope and context are set up, the stylesheet is defined.

        @selector ".fooClass", ->
            @font fonts.mono
            @bgColor bgColor
            @lineHeight @em 0.5
            @selector ".fooChildClass", ->
                @brush "sansPink"
                @size 12
        @selector ".spamClass", ->
            @color red
            @foo 105

If you ran that code, then `styles.tree` would evaluate to...

    {
        ".fooClass": {
            "font": "Ubuntu Mono",
            "background-color": "#EEEEFF",
            "line-height": "0.5em",
            ".fooChildClass": {
                "font": "sans",
                "color": "pink",
                "font-size": "12px"
            }
        },
        ".spamClass": {
            "color": "#FF0000",
            "font": "Comic Sans",
            "position": "fixed",
            "top": "0",
            "right": "0",
            "bottom": "105px",
            "left": "25px"
        }
    }

...and `styles.css` would evaluate to...

    .fooClass {
    font: Ubuntu Mono;
    background-color: #EEEEFF;
    line-height: 0.5em;
    .fooChildClass {
    font: sans;
    color: pink;
    font-size: 12px;
    }
    }
    .spamClass {
    color: #FF0000;
    font: Comic Sans;
    position: fixed;
    top: 0;
    right: 0;
    bottom: 105px;
    left: 25px;
    }