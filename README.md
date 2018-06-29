# xoxo

This is a very short (embedded) language for SuperCollider that uses [PreProcessor](https://github.com/supercollider-quarks/PreProcessor) Quark.  
My main (and only) goal was to make it as short as possible (both the rhythmic Patterns and the language itself).  
The whole implementation fits in 259 characters (so it fits in a twit... of 280 chars).  
Here's an expanded version:

```
this.preProcessor = PreProcessor.new.startDelimiter_("*:").endDelimiter_(":*");
x = (
  lang: \xoxo,
  languages: (
    xoxo: {
      |code, event|
      /*
      `code` is what we wrote inside `*: ... :*`, ie: "synthdef:xoxoxoxo"
      `event` is the current event
      */
      // So, it splits the string to obtain...
      var values = c.split($:);
      // which synthdef to play
      var instrument = v[0].asSymbol;
      // and when to play, translating 'xoxo' to '[1,0,1,0]' which is used by the `\amp` key.
      var pattern = values.at(1).asList.collect{ |xo| (xo.asString == "x").binaryValue }; 
      // then we put everything together in a Pbind
      var output = Pbind(
        \amp, Pseq(pattern, inf),
        \instrument, instrument,
        \dur, 1/4        
      );
      // which is embedded inside the Event in the `\xo` key.
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

### XOXO <-> SC

A `*:synth:xoxoxo:*` Pattern by itself won't play anything because basically it's just a Function (`x.at(\languages).at(\xoxo)`), so as we would do with any other Function, we need to call it passing the whole `x` Event, which will return the Event with a new `.xo` key (the Pbind).

```
            .-------------------------> 1. xoxo pattern
           /         .----------------> 2. call it passing `x` Event with this tiny lang implementation,
          /         /      .----------> 3. we get an Event with a `xo` key (which cointains the Pbind)
         /         /      /   .-------> 4. Pbind.play
        /         /      /   /
,------------,,--------,,-,,---,
*:synth:xoxo:*.value(x).xo.play;
```

### OBLIGATORY 4/4 PATTERN:
```
*:kick:xooo:*.value(x).xo.play;
*:hat:ooxo:*.value(x).xo.play;

// or even shorter version:

*:kick:xooo:*.(x).xo.play;
*:hat:ooxo:*.(x).xo.play;
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
