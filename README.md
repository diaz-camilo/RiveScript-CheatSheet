# RiveScript-CheatSheet

`!` definition

`+` trigger, expected input

`-` reply, output

`^` line continuation (doesn't imply new line)

`*` wildcard 

`#` number wildcard 

`_` word wildcard 

`<star>, <star1>, <star2>` refers to the wildcard value as entered by the user

`(...|...|...)` list of alternanives for the "wildcard", to refer to it in the reply still use `<star>`

`[...|...|...]` list of optionals, since they are optional, the can not be referenced by `<star>` 

`\n` new line

`//` in-line comment

`! local concat = newline | space | none` sets the `^` mode

`! sub i'm = i am` substitute i'm for i am before the RiveScript interpreter reads it

`! var foo = bar` -> declares a variable

`! array colors = red blue green "or" light blue|dark blue|medium blue` array, for single-word items, space is enough, multiple-word array items must be separated by pipes `|`

`(@colors) [@colors]` references the array in the trigger as alternatives or optionals respectively

`{@...}, {@<star>}, <@>` -> redirects to another response or responses. a reply can contain several redirections and they all be executed

`%` similar to the `+` used for triggers, except it looks at the bot's previous response to the user instead

`<set varName = value` or `<star#>` or `<formal> >` set variable value, use `<formal>` to maintain caps
`<get varName>` get variable value
`<bot varName>` get variable value as defined with `! var varName = value`

## Conditionals

`==`  equal to

`eq`  equal to (alias)

`!=`  not equal to

`ne`  not equal to (alias)

`<>`  not equal to (alias)

### for numbers only

`<`   less than

`<=`  less than or equal to

`>`   greater than

`>=`  greater than or equal to


`* <value> == <test value> => serve this reply` 

For example:

     + my name is *
     * <formal> == <bot name> => Wow, we have the same name!<set name=<formal>>
     * <get name> == undefined  => <set name=<formal>>Nice to meet you!
     - <set oldname=<get name>><set name=<formal>>
     ^ I thought your name was <get oldname>?

## Topics

`> topic newTopic` starts a new topic, once inside a topic, the only available triggers are those within the topic.

`< topic` closes the topic, topics must allways be closed.

`{topic=newTopic}` redirects to to topic with name "newTopic"

## Begin block

`> begin` Start the begin block

`+ request` Marks entry point

`{ok}` At this point the request leaves the begin block looking for triggers.

`< begin` End the begin block


    > begin

      // If we don't know their name, set the new_user topic and continue.
      + request
      * <get met> == undefined => <set met=true>{topic=new_user}{ok}
      - {ok}

    < begin

## Object Macros

`> object name language` Opens an onbject block. `language` can be pearl, Javascript, Python.

`args` the arguments array

`< object` Close object block.

    > object logical_OR javascript

        let result = false;
        const var1 = args[0];
        const var2 = args[1];

        if (var1 == "true" || var2 == "true"){
            result = true;
        }
        return result;

    < object

### Implementation example:

    ! version = 2.0

    > object find_final_bool javascript

        let result = false;
        const variable1 = args[0];
        const variable2 = args[1];

        if (variable1 == "true" || variable2 == "true"){
            result = true;
        }
        return result;

    < object

    + _ _
    * <call> find_final_bool <star1> <star2> </call> == true => <set final_bool=true>awesome {topic=random}
    - Failed 

    + *
    - Invalid input, try only two values.

## Globals

`! global debug = true | false` sets the debug mode

`! global depth = 50` sets the max number of redirects without a replay, to avoid infinite recursion.

## Person

`! person i am = you are` a susbtitution rule use for swapping first and second person pronouns

## Tags {...} {/...}

`{formal} {/formal}` or `<formal>`

`{sentence} {/sentence}`

`{uppercase} {/uppercase}`

`{lowercase} {/lowercase}`

`{person} {/person}` or `<person>`

`{topic=...}` Change the topic

`{weight=###}` in trigger, sets the matching priority. In reply, sets the reply's likelyhood of appearing.

`{@ ...} , <@>` redirects to a different trigger

`{random} ... {/random}` chose a random value

`{} {/}`

`<star>, <star1> - <starN>` references a `*` in the trigger.

`<botstar>, <botstar1> - <botstarN>` references a `*` in the bot's response.

    + i bought a new *
    - Oh? What color is your new <star>?

    + (@colors)
    % oh what color is your new *
    - <star> is a pretty color for a <botstar>.

`<input>, <input1> - <input9>` retrieves the previous 9 user inputs.

`<reply>, <reply1> - <reply9>` retrieves the previous 9 bot replys.

`<id>` user id as set by or passed to the RiveScript interpreter.

`<bot>` used to set or get a bot variable.

`<env>` used to set or get global variables.

`<get> , <set>` used to set or get user variables.




