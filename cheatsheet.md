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



## Tags {...} {/...}

`{formal} {/formal}`

`{random} {/random}`

`{sentence} {/sentence}`

`{uppercase} {/uppercase}`

`{lowercase} {/lowercase}`

`{} {/}`
