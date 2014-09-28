# Cascading Coffee Scripts

This is a work in progress. There needs to be a function for each CSS property.

    class CCS

        constructor: (realm, @css="") ->

            @stack = [{}]
            realm.apply @
            @tree = do @stack.pop

            @css += do stringify = (tree=@tree) ->

                css = ""
                for key, value of tree then css +=
                    if value is Object value then "\n#{key} {#{stringify value}\n}"
                    else "\n#{key}: #{value};"
                css

        selector: (selector, realm) ->

            @stack.push {}
            realm.apply @
            @merge @hash selector, do @stack.pop

        hash: (key, value) ->

            output = {}
            output[key] = value
            output

        merge: (hash) ->

            evaluate = (value) ->

                numeric = (value) ->

                    not (value instanceof Array) and
                    value - parseFloat(value) + 1 >= 0

                if value is 0 then "0"
                else if numeric value then "#{value}px"
                else value

            @stack[@stack.length-1][key] = evaluate value for key, value of hash

        # property functions...

        font:     (arg) -> @merge "font-family": arg
        size:     (arg) -> @merge "font-size": arg
        color:    (arg) -> @merge "color": arg
        bgColor:  (arg) -> @merge "background-color": arg
