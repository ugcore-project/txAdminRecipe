# Your resources

Put your own resources here, one folder each, then uncomment `ensure [local]` in `resources.cfg`.

They start after UgCore, so they can import it:

```lua
-- fxmanifest.lua
dependencies {
    'ug-core',
    'ug-lib',
}

shared_scripts {
    '@ug-core/import.lua',
    '@ug-lib/import.lua',
}
```

Start from [ug-template](https://github.com/ugcore-project/ug-template) for a resource that follows UgCore's conventions.
