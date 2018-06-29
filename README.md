# xoxo

This is a very short (embedded) language for SuperCollider. My main (and only) goal was to make it as short as possible (both the rhythmic Patterns and the language itself).  
The whole implementation fits in 259 characters. Here's an expanded version:

```
this.preProcessor = PreProcessor.new.startDelimiter_("*:").endDelimiter_(":*");
x = (
  lang: \xoxo,
  languages: (
    xoxo: {
      |code, event|
      var values = c.split($:);
      var instrument = v[0].asSymbol;
      var pattern = values.at(1).asList.collect{ |xo| (xo.asString == "x").binaryValue };
      var output = Pbind(
        \amp, Pseq(pattern, inf),
        \instrument, instrument,
        \dur, 1/4        
      );
      event.put(\xo, output);
    }
  )
);
```

## SYNTAX

The syntax is **really** basic:

* `*:` to open the pattern
* `synthdef:pattern` (x plays, o stays silent)
* `:*` to close the pattern

### OBLIGATORY 4/4 PATTERN:
```
*:kick:xooo:*.value(x).xo.play;
*:hat:ooxo:*.value(x).xo.play;
```

## LICENSE 

```
        DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE 
                    Version 2, December 2004 

 Copyright (C) 2018 Mauro L. <mauro@sdf.org> 

 Everyone is permitted to copy and distribute verbatim or modified 
 copies of this license document, and changing it is allowed as long 
 as the name is changed. 

            DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE 
   TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION 

  0. You just DO WHAT THE FUCK YOU WANT TO.
```
