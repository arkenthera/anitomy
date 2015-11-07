### Anitomy with DLL linking

Weirdly original author just put the code files on Git without any build system.
I just added CMake compatibility and DLL linking by exporting each class.Maybe there is no need to export everything but 
I dont have the time to iterate through the code.

```
#if defined(BuildingAnitomy)
#  define AnitomyExport __declspec( dllexport )
#else
#  define AnitomyExport __declspec( dllimport )
#endif

...

class AnitomyExport Anitomy
...
```

So you can build Anitomy as seperate target and link it against your library.

See original repo [here](https://github.com/erengy/anitomy)
