This takes JSDuck output (https://github.com/senchalabs/jsduck) and turns it into type declarations for Typescript (version 1.4+). 
Forked from https://github.com/Dretch/typescript-declarations-for-ext I modified to include:
 * Configuration created in a new interface name `<Ext class name>Config` and typed as the argument for the `any` constructor if 
   any config elements exist for that class.  To add new configuration to an extended class you would simply create a new interface 
   to define the config:

```
module MyModule {
	interface MyPanelConfig extends Ext.panel.PanelConfig {
		customConfig?: string
	}
	
	export class MyPanel extends Ext.panel.Panel {
		MyPanel(cfg: MyPanelConfig) {
		super(cfg);
		}
	}
}
```

 * Short documentation emitted for inline help

This is designed to be used in conjunction with my patched Typescript compiler that emits ExtJS style classes 
(https://github.com/revelation0/TypeScript/tree/module-extjs). 


Pre-generated declaration files
===============================

There is a pre-generated declarations files currently only for Ext 5.1.2 in the declarations folder. Make an issue if you would 
like to see another one, or generate it yourself (see below).


Generating your own declaration file
====================================

I have not yet updated the generate-defaults file, use at your own risk.  For 5.1.2 I extracted the source but had to exclude
packages/sencha-soap as it was causing errors with jsduck to generate the json for consumption.  My process right now is:

Compile the generator:

```tsc --module commonjs generator.ts```

Then move into the folder that contains the ExtJS source and build the docs:

```jsduck ext-5.1.2/src/ ext-5.1.2/packages/sencha* --export=full --output docs```

and then use the docs folder to generate the declarations file:

```node ../typescript-declarations-for-ext/generator.js ./docs ./extjs-5.1.2.d.ts```