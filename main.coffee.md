# Cascading Coffee Scripts

This is a work in progress; the property functions need extending.

      class CCS

        constructor: (styles, @css="") ->

            @brushes = {}

            @stack = [{}]
            styles.apply @
            @tree = do @stack.pop

            @css += do stringify = (tree=@tree) ->

                css = ""
                for key, value of tree then css +=
                    if value is Object value then "\n#{key} {#{stringify value}\n}"
                    else "\n#{key}: #{value};"
                css

        selector: (selector, styles) ->

            @stack.push {}
            styles.apply @
            @merge @hash selector, do @stack.pop

        brush: (name, styles=undefined) ->

            if styles then @brushes[name] = styles
            else @brushes[name].apply @

        hash: (key, value) -> output = {}; output[key] = value; output

        merge: (hash) ->

            evaluate = (value) ->

                numeric = (value) ->

                    not (value instanceof Array) and
                    value - parseFloat(value) + 1 >= 0

                if value is 0 then "0"
                else if numeric value then "#{value}px"
                else value

            @stack[@stack.length-1][key] = evaluate value for key, value of hash

        # font...
        font:     (arg) -> @merge "font": arg
        family:   (arg) -> @merge "font-family": arg
        size:     (arg) -> @merge "font-size": arg
        color:    (arg) -> @merge "color": arg
        weight:   (arg) -> @merge "font-weight": arg
        style:    (arg) -> @merge "font-style": arg
        variant:  (arg) -> @merge "font-variant": arg

        # text...
        lineHeight:     (arg) -> @merge "line-height": arg
        letterSpacing:  (arg) -> @merge "letter-spacing": arg
        wordSpacing:    (arg) -> @merge "word-spacing": arg
        textAlign:      (arg) -> @merge "text-align": arg
        textDecoration: (arg) -> @merge "text-decoration": arg
        textIndent:     (arg) -> @merge "text-indent": arg
        textTransform:  (arg) -> @merge "text-transform": arg
        verticalAlign:  (arg) -> @merge "vertical-align": arg
        whiteSpace:     (arg) -> @merge "white-space": arg

        # background...
        background:   (arg) -> @merge "background": arg
        bgColor:      (arg) -> @merge "background-color": arg
        bgImage:      (arg) -> @merge "background-image": arg
        bgRepeat:     (arg) -> @merge "background-repeat": arg
        bgPosition:   (arg) -> @merge "background-position": arg
        bgAttachment: (arg) -> @merge "background-attachment": arg

        # helpers...

        em: (arg) -> "#{arg}em"

        setPosition: (position, top, right, bottom, left) -> @merge
            position: position
            top: top
            right: right
            bottom: bottom
            left: left