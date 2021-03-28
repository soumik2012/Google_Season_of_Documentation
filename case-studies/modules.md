# Metric and carbon calculation modules

Author: Shubham K

## Table of Contents

* [Goal](#goal)
* [Glossary](#glossary)
* [Background](#background)
* [What are modules?](#what-are-modules)
* [What type of modules are there?](#types-of-modules)
* [Why are modules important?](#importance-of-modules)
* [How do we import a module?](#importing-modules)
* [How are modules configured during a FLINT run?](#configuring-modules)
* [How do modules interact?](#interacting-modules)
* [Developing modules](#developing-modules)
* [What are the typical components of a module?](#module-components)
* [How can existing modules be combined?](#combining-modules)
* [How do I create a new module?](#creating-modules)
* [Translating a system of equations into C++](#process-model)
* [Wrapping existing models](#foriegn-functions)
* [How do I make a point-based module spatially explicit?](#spatial-modules)
* [Creating custom modules](#custom-modules)
* [What are Disturbance Events?](#disturbance-events)
* [Key Components of the FLINT](#flint-namespace)

## Goal

Demonstrate how to implement a single, simple module from scratch.

This will include:
issues to consider when starting a module
the boilerplate for a blank module,
the source and header files that describe the process of the module,
the connections and registration of the module to the FLINT engine
setting default configuration and test dataset
calling the module from the command line

Further extensions will include running the module spatially, passing external data and new configuration, and linking it with other modules, but this may happen at a later date.

## Glossary

* DynamicVariable & DynamicObject : Derived from Portable Components (POCO) library in dynamic.h. They are used to convert the data from one type to another with minimum or no data loss. More info: http://bit.ly/3ddglyr

* override : C++ keyword used to match the function definition with the module function definition in the module base header file.
extern: The extern "C" keyword tells a C++ compiler not to mangle (rename)

* C function names. It changes the linkage of a function in such a way that the function is callable from C. In practice that means that the function name is not mangled.


## Background
### What are modules?

Modules are the building block of FLINT. These contain the operations, describing the ecological processes and driving the carbon changes in the landscape.

There are multiple types of modules in FLINTpro to perform different functions. For the sake of simplicity, we use the term module in this document to mean calculation modules. Other modules are covered in other documents.

This document describes how to create FLINT modules for calculating metrics such as carbon and GHG emissions. These modules can come in all different forms.

### What type of modules are there?

* Point process
* Spatially explicit
* Unlinked - driven by input data
* Linked - driven by multiple modules and associated parameters

### Why are modules important?

Without calculation modules, the FLINT cannot produce outputs. Each user will have different data and policy and reporting needs. Based on these needs users will attach the appropriate modules and data to the FLINT.
Given the wide range of multiple

### How do we import a module?

Two common places to find modules:
* Modules in main FLINT repo
* Custom modules, developed by users

The second half of this document describes creating a custom module.

### How are modules configured during a FLINT run?

### How do modules interact?

------

## Developing modules
### What are the typical components of a module?

There are three main components of a module

* Input data: For each calculation the
* Algorithms: The equations that are used to calculate the stocks and flows of the metric
* Output data: Data results handed back to the FLINT following the calculation.

RothC might be good to start or perhaps Chapman Richards (or both!)

### How can existing modules be combined?

Ideally modules are separated to the simplest functions. For example, it is typically desirable to break a complex model down into several smaller modules as this will make modification and changes easier in the future. It also allows for components to be more easily reused across multiple applications of the FLINT.

Combining modules occurs in three main ways:
Linking through the stocks/pools being record in the FLINT
Setting the order of operation of modules in a time step
That is, what modules need to run in what order for the system to work.
Linking all modules to the same core input data and event triggering systems
Under all circumstances, all modules will be subscribed to the same event triggering system to avoid errors of omission and commission.

Modules that act in isolation are those that 1) work on their own pools and stocks (i.e., no other modules interact with the stocks or flows) and 2) do not interact with other modules. A good example of these modules are emissions factor approaches for non-CO2 emissions. For example, N2O emissions from fertiliser application is often calculated simply as a percentage of the amount of N applied by type. They do not consider the N status of the soil or other factors.

In more advanced systems it is highly likely that modules will interact in at least some way. For mass-balance type methods multiple modules will operate on the same set of pools. For example, a dead organic matter pool (DOM) [insert diagram] will have carbon added from modules that calculate plant turnover rates and disturbances (e.g. harvesting), the DOM module will then calculate the losses due to decomposition, with the outputs from the pool then applied in both the atmosphere and a soil carbon module.

In the example above, the order of operation within a timestep is important. The results from the DOM module operations will depend on if they operate on the DOM pools before or after the new carbon is added from the plants.  [NEED TO DESCRIBE HOW TO SET THIS]
How do I create a new module?

Firstly it is important to ensure that there is not an existing module that can be used, either in its current state or with minimal modification. Existing modules are available on the moja global github account.

Where an entirely new module is required, there are four initial high-level steps
1. Based on user needs, identify the required model/s
2. Identify all the required input data
3. Including parameters, variables and if the data needs to be spatial, spatially referenced or aspatial.
4. Identify the pools the module will operate on.

Describe the development environment for editing modules
a template for the minimum viable module testing and debugging

> This is what Max said in an email: What I’ve noticed from the training workshops we put on is that most of the people who are interested in using the FLINT are not very technical, so the guide should probably try to avoid really technical language or unnecessary code-level details except for the essentials needed for writing a module… or at least include a chapter that gives higher-level instructions to avoid overwhelming people. I think a step-by-step tutorial on how to take FLINT.example and turn it into a simple custom module would go a long way, sort of like how the GCBM hands-on exercise is structured.

### Choosing a language

The majority of the FLINT is written in C++. This was a core design decision to allow for the fast processing required when running high spatial/temporal resolution modules.

However, many scientists are not comfortable with C++ and would prefer to develop and improve their models in languages such as Python and R. In this case it is possible to wrap the models. This will have performance implications, but is often suited for testing.  

### Translating a system of equations into C++

Most modules come from a scientific paper with equations that need to be transcribed into something FLINT can read. Usually, they have inputs, parameters and results.

How are each of these declared in C++ and how are they combined by FLINT to reproduce the same model in the paper?

### Wrapping existing models

[do we need this? Noting we can wrap python or the like? Mal would be best to chat to here]

FLINT can be used to make familiar models spatially explicit. The module template can be used to make calls to R or Python models, then feed the results back into FLINT. This allows for researchers to parameterise their model in a familiar environment, while leveraging the spatially-explicit framework of FLINT.

For large runs, calling external libraries will always be slower than transcribing the model to C++. We therefore focus on writing C++ in this document but will revisit this section in the future.

### How do I make a point-based module spatially explicit?

------

## Creating custom modules

> TODO: right now I am not sure of the order of the steps

### Registering module in libraryfactory.cpp

Import the library factory header file (`libraryfactory.h`) and all the headers of the included transforms and modules.

We need to register a module using **CMake** from `CMakeLists.txt`. Define a `MODULE_API` which will be referenced in all the header files present in the module.

Starting from `FLINT.example\Source\moja.flint.example.base\CMakeLists.txt` as a scaffold copy it to `moja.flint.modulename/CMakeLists.txt`, and make the following changes:

* Rename the `set(PACKAGE "base")` to  `set(PACKAGE "CustomModuleName")`

* Under `set(PROJECT_MODULE_HEADERS)` add all the module header files like `landusemodule.h`

```
set(PROJECT_MODULE_HEADERS
	#include/moja/flint/example/${PACKAGE}/CustomModuleName.h
	#include/moja/flint/example/${PACKAGE}/errorscreenwriter.h
  include/moja/flint/example/${PACKAGE}/landusemodule.h
)
```

* Under `set(PROJECT_MODULE_SOURCES)` add all the module source code files like `landusemodule.cpp`

```
set(PROJECT_MODULE_SOURCES
  src/CustomModuleName.cpp
  src/errorscreenwriter.cpp
  src/landusemodule.cpp
)
```


* Add the transform header files under `set(PROJECT_TRANSFORM_HEADERS)`

```
set(PROJECT_TRANSFORM_HEADERS
  include/moja/modules/${PACKAGE}/hansenforestcovertransform.h
)
```


* And add the transform source code files under

```
set(PROJECT_TRANSFORM_SOURCES
	src/xxx.cpp
  src/hansenforestcovertransform.cpp
)
```

> TODO: need to make a CMake guide with screenshots

* In `libraryfactory.h`, edit the `#define` and include the `_modules.ModuleName_exports.h` file with the `librarymanager.h` file and add `#endif` at the end.

* Edit the `other/libraryfactory.cpp`. Include the `libraryfactory.h` header and headers of all the included modules (e.g. **ExampleModule**), disturbance events and transforms.

* For every module you need to add this line (in this example we are adding the **DisturbanceEventModule** )

  ````
  outModuleRegistrations[index++] = ModuleRegistration{
  	"DisturbanceEventModule", []() -> flint::IModule* {
      	return new DisturbanceEventModule();
      }
  };
  ````

  to register a module under `MOJA_LIB_API int getModuleRegistrations(ModuleRegistration* outModuleRegistrations)`.

* And similarly for all included transforms,add:

```c++
outTransformRegistrations[index++] = TransformRegistration{
    "TimeSeriesTransform",[]() -> flint::ITransform* {
        return new TimeSeriesTransform();
    }
};
```

 	under `MOJA_LIB_API int getTransformRegistrations(TransformRegistration* outTransformRegistrations)`

This code will create an instance of the `IModule` class or `ITransform` class.



### LibraryFactory

The purpose of the LibraryFactory is to tell the framework what to construct (and how to construct it) when one of those JSON config files asks for something by its key.

Eg. To use `LocationIdxFromFlintDataTransform` we write the code

```C++
  outTransformRegistrations[index++] =
      TransformRegistration{"LocationIdxFromFlintDataTransform",
                            []() -> flint::ITransform* {
                                return new LocationIdxFromFlintDataTransform();
                            }
                           };
```

and then the JSON config file would look like

```json
...
"initial_age": {
    "transform": {
        "library": "internal.flint",
        "type": "LocationIdxFromFlintDataTransform",
        "provider": "RasterTiled",
        "data_id": "initial_age"
    }
},
...
```

Finally the file should look somewhat like this:

```c++
// Instance of common data structure
extern "C" {
	MOJA_LIB_API int getModuleRegistrations
        (ModuleRegistration* outModuleRegistrations) {
			int index = 0;
			outModuleRegistrations[index++] = ModuleRegistration{
                "ExampleModule", []() -> flint::IModule* {
                    return new ExampleModule();
			}
		};
		return index;
    }
	MOJA_LIB_API int getTransformRegistrations
        (TransformRegistration* outTransformRegistrations) {
		int index = 0;
		outTransformRegistrations[index++] = TransformRegistration{
            "ExampleTransform",	[]() -> flint::ITransform* {
                return new ExampleTransform();
            }
        };
		return index;
	}
	MOJA_LIB_API int getFlintDataRegistrations
        (FlintDataRegistration* outFlintDataRegistrations) {
		auto index = 0;
		return index;
	}
	MOJA_LIB_API int getFlintDataFactoryRegistrations
        (FlintDataFactoryRegistration* outFlintDataFactoryRegistrations) {
		auto index = 0;
		return index;
	}
	MOJA_LIB_API int getDataRepositoryProviderRegistrations
        (moja::flint::DataRepositoryProviderRegistration* outDataRepositoryProviderRegistration) {
		auto index = 0;
		return index;
	}
}
```



### FlintData

`FlintData` to be honest I don't often use, but it's basically a more strongly typed kind of variable that doesn't do so much packing and unpacking of Dynamics.


```c++
MOJA_LIB_API int getFlintDataRegistrations
    (FlintDataRegistration* outFlintDataRegistrations) {
    auto index = 0;
    outFlintDataRegistrations[index++] = FlintDataRegistration{
        "RunStatistics", []() -> flint::IFlintData* {
            return new RunStatistics();
        }
    };
    return index;
}

// OR
MOJA_LIB_API int getFlintDataFactoryRegistrations
    (FlintDataFactoryRegistration* outFlintDataFactoryRegistrations) {
	auto index = 0;
	return index;
}
```

### Data providers

`FlintDataRepositoryProviderRegistration` is used to import different types of data through a data provider. Eg the optional `moja.modules.gdal ` package implements a provider for reading from rasters using GDAL

The thing they all have in common is that first string `arg`, which is what links the code to the JSON config files; for example, in `moja.modules.gdal` `libraryfactory.cpp` file, you'll see this:

```c++
outDataRepositoryProviderRegistration[index++] = DataRepositoryProviderRegistration{
      "RasterTiledGDAL", static_cast<int>(datarepository::ProviderTypes::Raster),
      [](const DynamicObject& settings) -> std::shared_ptr<datarepository::IProviderInterface> {
         return std::make_shared<datarepository::ProviderSpatialRasterTiled>(
             std::make_shared<RasterReaderFactoryGDAL>(), settings);
      }
};
```

And then a simulation could use that in its provider JSON config file (the "type" and "library" parts are the relevant bits - note how the "type" value matches the name in the `DataRepositoryProviderRegistration`):

```json
{
   "Providers": {
       "Spatial": {
           "layers": [
               {
                   "name": "initial_age",
                   "layer_path": "..\\layers\\tiled\\initial_age_moja.tiff",
                   "layer_prefix": "initial_age_moja"
               }
           ],
           "blockLonSize": 0.1,
           "tileLatSize": 1.0,
           "tileLonSize": 1.0,
           "cellLatSize": 0.00025,
           "cellLonSize": 0.00025,
           "blockLatSize": 0.1,
           "type": "RasterTiledGDAL",
           "library": "moja.modules.gdal"
       }
   }
}
```



### Create module.h

After registering the modules, we can start writing the module specific code. We will start with importing the necessary header files first which are `#include <moja/flint/modulebase.h>` and  `#include _modules.ModuleName_exports.h` followed by module specific files required according to the use case of the module.

In the case of our ExampleModule, the `ExampleModule.h` file should look like this:

Import modulebase.h.

```c++
#include <moja/flint/modulebase.h>
```

Declare the moja → flint → example → examplemodule namespace

```c++
namespace moja {
namespace flint {
namespace example {
namespace examplemodule{
```

Create an ExampleModule class with `EXAMPLE_MODULE_API` which was specified earlier in   `_modules.ExampleModule_exports.h`  and extend the class from the ModuleBase class in the FLINT class followed by initialising the newly created class with its default constructor and using virtual to refer to this particular class.

```c++
// use code blocks instead of screenshots
```

All the variables declared in this class will be used in the module CPP file.
In the example screenshots, variables and members defined in `rothcmodule.h` are used in rothcmodule.cpp.

```c++
class EXAMPLE_MODULE_API ExampleModule : public flint::ModuleBase {
   public :
      ExampleModule () = default;    
    virtual ~ExampleModule () = default;
```



#### Configure

Next declare the `configure()` function. This method is used to pass extra information from the config files directly to a module. By changing the values in the JSON config files, we can modify the simulation and events in different ways. The FLINT will read the config files and make the changes in the current simulation accordingly.

For example:

```
void configure(const DynamicObject&) override;
```

The `subscribe()` function will connect the module to the notification handler and details of every event will be passed to the module. The module can then use these details in the simulation.

e.g. If a `SprayFertiliser()` event takes place then the module will be alerted and it can use the passed values for calculations of the soil.

```c++
void subscribe(NotificationCenter& notificationCenter) override;
```

#### Timing controllers

`onLocalDomainInit()` & `onPreTimingSequence() ` are functions from `imodule.h` library which will run some code at specific time.

For example `onPreTimingSequence()` will run before time starts in the simulation. The code to be run has to be written in the appropriate function. More functions are available which can be used to customise the way you want to run at specific times or events.

```c++
void onLocalDomainInit() override;
void onPreTimingSequence() override;
```



#### Declare process model variables

Under the private access specifier, define all the variables that will be used in the module development (depends on the science).

```c++
private:
	double a;
	double b;
      double temp;
	const flint::IPool* _PoolA;
	const flint::IPool* _PoolB;
	const flint::IPool* _PoolC;
      double ratio_1;
      double ratio_2;
	const flint::IVariable* _presCM;
```



Finally close all the namespaces

````c++
			}
		}
	}
}
````



### Create module.cpp

In the module.cpp file:

* Import all the necessary headers and namespaces
* Call the configure() function
* Call the subscribe function to connect the module to the NotificationCenter.

``` c++
void ExampleModule ::subscribe(NotificationCenter& notificationCenter) {
	notificationCenter.subscribe(signals::TimingInit, &ExampleModule ::onTimingInit, *this);
	notificationCenter.subscribe(signals::TimingStep, &ExampleModule ::onTimingStep, *this);
}
```

* In the example, the ExampleModule will be notified with the signals
  * `onTimingInit` - On start of the simulation (before time starts)
  * `onTimingStep` - On each timing step (eg. month or days)



### FLINT.example

Now let's follow and understand the above steps with reference to the base test module included in  https://github.com/moja-global/FLINT.Example

#### LibraryFactory

##### Header

From `moja.flint.example.base/include/moja/flint/example/base/libraryfactory.h`

```c++
#ifndef MOJA_FLINT_EXAMPLE_BASE_LIBRARYFACTORY_H_
#define MOJA_FLINT_EXAMPLE_BASE_LIBRARYFACTORY_H_
#pragma once

#include "moja/flint/example/base/_modules.base_exports.h"
#include <moja/flint/librarymanager.h>

namespace moja {
namespace flint {
namespace example {
namespace base {

// Instance of common data structure
// The extern "C" keyword tells a C++ compiler not to mangle (rename) C function names.
extern "C" MOJA_LIB_API int getModuleRegistrations(moja::flint::ModuleRegistration* outModuleRegistrations);
extern "C" MOJA_LIB_API int getTransformRegistrations(moja::flint::TransformRegistration* outTransformRegistrations);
extern "C" MOJA_LIB_API int getFlintDataRegistrations(moja::flint::FlintDataRegistration* outFlintDataRegistrations);
extern "C" MOJA_LIB_API int getFlintDataFactoryRegistrations
    (moja::flint::FlintDataFactoryRegistration*	outFlintDataFactoryRegistrations);
extern "C" MOJA_LIB_API int getDataRepositoryProviderRegistrations
    (moja::flint::DataRepositoryProviderRegistration* outDataRepositoryProviderRegistration);

}}}} // moja::flint::example::base

#endif	// MOJA_FLINT_EXAMPLE_BASE_LIBRARYFACTORY_H_
```

##### Source

From  `moja.flint.example.base/src/libraryfactory.cpp`

Required imports

```c++
#include "moja/flint/example/base/libraryfactory.h"
#include "moja/flint/example/base/errorscreenwriter.h"
#include "moja/flint/example/base/timeseriestransform.h"
using moja::flint::IModule;
using moja::flint::ITransform;
using moja::flint::IFlintData;
using moja::flint::ModuleRegistration;
using moja::flint::TransformRegistration;
using moja::flint::FlintDataRegistration;
using moja::flint::FlintDataFactoryRegistration;
using moja::flint::DataRepositoryProviderRegistration;
```

Open namespace

```
namespace moja { namespace flint { namespace example { namespace base {
```



Register modules

Create a getModuleRegistrations() function which registers and initializes the module in a queue outModuleRegistrations,and we track the current index of the queue with the index variable.

> TODO: //please correct me if wrong

We use the flint::IModule* class to derive an instance of the ErrorScreenWriter, and pass it through ModuleRegistration structure (it takes the module name and the module initializer pointer) and then insert it at the end in the outModuleRegistrations queue with outModuleRegistrations[index++].

```c++
extern "C" {
	MOJA_LIB_API int getModuleRegistrations(ModuleRegistration* outModuleRegistrations) {
			int index = 0;
			outModuleRegistrations[index++] = ModuleRegistration{ "ErrorScreenWriter", []() -> flint::IModule* { return new ErrorScreenWriter(); } };
			return index;
		}
...
```

Now similarly register a Transform using the getTransformRegistrations which is derived from flint::ITransform*class and insert it in the outTransformRegistrations queue. In the example we register the CompositeTransform. The registration code is commented out.

```c++
MOJA_LIB_API int getTransformRegistrations(TransformRegistration* outTransformRegistrations) {
		int index = 0;
		outTransformRegistrations[index++] = TransformRegistration{ "TimeSeriesTransform",	[]() -> flint::ITransform* { return new TimeSeriesTransform(); } };
		return index;
	}
```

> TODO: need help to explain these 3 code blocks

```c++
MOJA_LIB_API int getFlintDataRegistrations(FlintDataRegistration* outFlintDataRegistrations) {
	auto index = 0;
	//outFlintDataRegistrations[index++] = FlintDataRegistration{ "RunStatistics", []() -> flint::IFlintData* { return new RunStatistics(); } };
	return index;
}

MOJA_LIB_API int getFlintDataFactoryRegistrations(FlintDataFactoryRegistration* outFlintDataFactoryRegistrations) {
	auto index = 0;
	return index;
}
```

> The main code is commented out, should we include it in the docs or not?

```c++
MOJA_LIB_API int getDataRepositoryProviderRegistrations(moja::flint::DataRepositoryProviderRegistration* outDataRepositoryProviderRegistration) {
		auto index = 0;
		//outDataRepositoryProviderRegistration[index++] = DataRepositoryProviderRegistration{ "RasterTiledBeast", static_cast<int>(datarepository::ProviderTypes::Raster), [](const DynamicObject& settings) ->std::shared_ptr<datarepository::IProviderInterface> { return std::make_shared<datarepository::ProviderSpatialRasterTiled>(std::make_shared<RasterReaderFactoryBeast>(), settings); } };
		return index;
	}
```

#### TestModule

##### Header

From `moja.flint/include/moja/flint/testmodule.h`

Required imports

```c++
#ifndef MOJA_FLINT_TESTMODULE1_H_
#define MOJA_FLINT_TESTMODULE1_H_

#include "moja/flint/ioperationresult.h"
#include "moja/flint/ipool.h"
#include "moja/flint/modulebase.h"
```

Open namespace

```c++
namespace moja {
namespace flint {
```

Create class FLINT_API from the MoudleBase class:
Under the public access specifier Initialise the default constructor.

```c++
class FLINT_API TestModule : public ModuleBase {
  public:
		TestModule() = default;
```

Initialise the default destructor.

```c++
virtual ~TestModule() = default;
```

Declare the configure function and the subscribe function.
```c++
void configure(const DynamicObject& config) override;
void subscribe(NotificationCenter& notificationCenter) override;
```

Declare the onLocalDomainInit, onTimingInit, onTimingStep functions.  
```c++
 void onLocalDomainInit() override;
 void onTimingInit() override;
 void onTimingStep() override;
```

Under the private access specifier :
Declare the pools

```c++
  private:
		const flint::IPool* _pool1;
		const flint::IPool* _pool2;
		const flint::IPool* _pool3;
```

Declare FLINT variables
```c++
	const flint::IVariable* _variable1;
	const flint::IVariable* _variable2;
	const flint::IVariable* _variable3;
```


Declare settings to be taken from the config files
These variables refer to the ratio of the carbon transfer

```c++
double ratio_1;
double ratio_2;
double ratio_3;
```

Declare additional C++ variables for variables and pools

```c++
 std::string variable_1;
 std::string variable_2;
 std::string variable_3;
 std::string pool_1;
 std::string pool_2;
 std::string pool_3;
};
}  // namespace flint
}  // namespace moja
#endif  // MOJA_FLINT_TESTMODULE1_H_
```

##### Source
From `moja.flint/src/testmodule.cpp`

Required imports

```c++
#include "moja/flint/testmodule.h"
#include "moja/flint/ilandunitdatawrapper.h"
#include "moja/flint/ioperation.h"
#include "moja/flint/ivariable.h"
#include <moja/notificationcenter.h>
#include <moja/signals.h>
```

Open namespace

```c++
namespace moja {
namespace flint {
```

Make the configure function to get the data from the config files and pass them to the FLINT simulation variables

```c++
void TestModule::configure(const DynamicObject& config) {
```

Define variables

```c++  
   ratio_1 = 0.50;
   ratio_2 = 0.50;
   ratio_3 = 0.50;

   variable_1 = "variable 1";
   variable_2 = "variable 2";
   variable_3 = "variable 3";

   pool_1 = "Pool 1";
   pool_2 = "Pool 2";
   pool_3 = "Pool 3";
```

Now we have 2 possibilities :
1. We specify the values of the variables in the config file.
In this case the values need to be passed from the config `DynamicObject` (which holds the whole config JSON file) to the variables in the simulation.
2. We DO NOT specify the values of the variables in the config file.
In this case, FLINT will use the default values defined above.

If the config file contains values specified for `ratio_1`, replace the default value of ratio_1 with the one in the config file.

```c++
if (config.contains("ratio_1")) {
      ratio_1 = config["ratio_1"];
   }
```

Similarly, do the same with `ratio_2` and `ratio_3`

```c++
if (config.contains("ratio_2")) {
      ratio_2 = config["ratio_2"];
   }
if (config.contains("ratio_3")) {
      ratio_3 = config["ratio_3"];
   }
```


Similarly do the same with variable_1, variable_2, variable_3 and pool_1, pool_2, pool_3.  

```c++
extract<const std::string>();  converts the JSON to readable C++ string as all the variables are of string data type.
   if (config.contains("variable_1")) {
      variable_1 = config["variable_1"].extract<const std::string>();
   }
   if (config.contains("variable_2")) {
      variable_2 = config["variable_2"].extract<const std::string>();
   }
   if (config.contains("variable_3")) {
      variable_3 = config["variable_3"].extract<const std::string>();
   }
//-------------------------------------------------------------------------
   if (config.contains("pool_1")) {
      pool_1 = config["pool_1"].extract<const std::string>();
   }
   if (config.contains("pool_2")) {
      pool_2 = config["pool_2"].extract<const std::string>();
   }
   if (config.contains("pool_3")) {
      pool_3 = config["pool_3"].extract<const std::string>();
   }
```

Make a subscribe function from the TestModule class which takes a NotificationCenter pointer called notificationCenter as input.

```c++
void TestModule::subscribe(NotificationCenter& notificationCenter) {
```

Now connect the notificationCenter pointer to the LocalDomainInit  from the signals class (signals), and the reference to our onLocalDomainInit function from our TestModule class (&TestModule::onLocalDomainInit) and this pointer (*this) which refers to this subscribe function itself.

```c++
notificationCenter.subscribe(signals::LocalDomainInit, &TestModule::onLocalDomainInit, *this);
```

Similarly define functions for the TimingInit & TimingStep signals.

```c++
 notificationCenter.subscribe(signals::TimingInit, &TestModule::onTimingInit, *this);
 notificationCenter.subscribe(signals::TimingStep, TestModule::onTimingStep, *this);
}
```

Now create the onLocalDomainInit function

```c++
void TestModule::onLocalDomainInit() {
```

Get the values of pool_1 from the _landUnitData object with the getPool function and assign it to _pool1 variable in our simulation.

```c++
   _pool1 = _landUnitData->getPool(pool_1);
   _pool2 = _landUnitData->getPool(pool_2);
   _pool3 = _landUnitData->getPool(pool_3);
```

Now get the values of _variable1 from the _landUnitData object with the getVariable function and assign it to variable_1 variable in our simulation.

```c++
   _variable1 = _landUnitData->getVariable(variable_1);
	_variable2 = _landUnitData->getVariable(variable_2);
	_variable3 = _landUnitData->getVariable(variable_3);
```

Declare the onTimingInit function

```c++
void TestModule::onTimingInit() {} //I have no idea why is it empty?????? Need help in this
```

Declare the onTimingStep function. This function will run at every TimeStep.

```c++
// need suitable science example in practice
void TestModule::onTimingStep() {
```

From _landUnitData, call the createProportionalOperation() function (it will return a pointer which points to the LandUnitWrapper and hence directly connects to the input data of the current simulation). This function will create an operation called Proportional Operation.
More info about Operations in FLINT here: 1.2 Operations · moja-global/FLINT Wiki

Create a variable, operation of the auto type (i.e. C++ will set the type of this variable for you) and set it to the _landUnitData->createProportionalOperation(). Hence we have a
Proportional Operation and stored it in the operation variable.

```c++
 auto operation = _landUnitData->createProportionalOperation();
```

Now we use the operation to add a Transfer of carbon Pool 1 to Pool 2 in some ratio (e.g. if ratio=0.5, 50% of total carbon in Pool 1 is transferred to Pool 2.) using the addTransfer function.
This line of code will run addTransfer() with _pool1 and _pool2 as the source and destination pools respectively and transfer (ratio_2 * total carbon of pool_1) from _pool1 to _pool2.

```c++
operation->addTransfer(_pool1, _pool2, ratio_1)
```

Similarly this line of code will transfer  (ratio_2 * total carbon of pool_2) from _pool2 to _pool3.

```c++
->addTransfer(_pool2, _pool3, ratio_2)
```

And thus this final line of code will transfer  (ratio_3 * total carbon of pool_3) from _pool3 to _pool1. The semicolon at the end will complete the operation sequence. The operations are run in the order they are defined in code.

```c++
->addTransfer(_pool3, _pool1, ratio_3);
```

Finally use the submitOperation of the _landUnitData to submit our operation variable of the type (ProportionalOperation) to the FLINT LandUnitController to run the operations on the input data.

Finally close the onTimingStep() function And in the end close the necessary namespaces

```c++
}
}  // namespace flint
}  // namespace moja
```

//incomplete
// TODO: write a guide on how to connect all this code to the config file

### Configuration files in FLINT

While running moja.cli, we pass the config file as a parameter to the FLINT. The config variable used in the module source code is of the DynamicObject type (a type from POCO library). This config variable holds the whole JSON config file and then is used to modify the simulation.
e.g. This is a config JSON file


```json
{
  "eventqueue": {
    "flintdata": {
      "library": "internal.flint",
      "type": "EventQueue",
      "settings": {
        "events": [
          {
            "date": {
              "$date": "2000/01/01"
            },
            "id": 1,
            "type": "agri.NFertEvent",
            "name": "Synthetic fertilizer",
            "quantity": 100,
            "runtime": 5
          },
          {
            "date": {
              "$date": "2001/05/01"
            },
            "id": 2,
            "type": "agri.NFertEvent",
            "name": "Organic fertilizer",
            "quantity": 200,
            "runtime": 5
          },
```

Some parts of the config files (especially involving flintdata as it is a generic data type to hold objects) may be exclusive to a particular module implementation.

Explanation of terms
* library : The library to be used (eg. moja.modules.cbm (GCBM), moja.flint.example.agri (agricultural soil module), internal.flint (FLINT Core library))
* "type": "EventQueue" : Type of the variable inside flintdata/ EventQueue  It will be an event if it is inside an EventQueue like ( agri.NFertEvent Fertilising event),else it can be any variable type as flintdata supports all kinds of C++ objects like    SpatialLocationInfo  or transforms like CompositeTimeSeriesTransform.
* "id"  / "order" : The position of the event in the Event Queue
* "Name": Name of the event/operation to be used. Eg. Plant Dryland Forests (chapman_richards.ForestPlantEvent), Simple(operationManager), Manure Management Event (agri.ManureManagementEvent),etc.
* "quantity": Quantity of stock units (could be carbon, nitrogen,etc.)
* "runtime" : ??????//need help here
* "settings": Usually refers to flags related to FLINT runs. Eg.         	* "debugging_enabled" can be used to enable/disable debugging.
*  "Variables": This object holds all kinds of FLINT variables like "ipcc_climate_zone" or "region"

And this is the method from the module CPP file.

```c++
void NFertEvent::configure(DynamicObject config, const flint::ILandUnitController& landUnitController, datarepository::DataRepository& dataRepository) {
   DisturbanceEventBase::configure(config, landUnitController, dataRepository);
   quantity = config["quantity"];
   runtime = config["runtime"];
}
```

In this way we are able to set the quantity and the runtime of the Synthetic fertiliser to 100 and 5 and the organic fertiliser to 200 and 5 respectively.

Another example; The JSON config file:

```json
 "CBMGrowthModule": {
        	"enabled": true,
        	"order": 7,
        	"library": "moja.modules.cbm",
        	"settings": {
            	"debugging_enabled": true
       	 }    	},
```

Where everything in the optional “settings” section is what gets passed as the argument to void configure(const DynamicObject&) (which was declared in the module header file).
Then the growth module code looks like:   

```c++
 void YieldTableGrowthModule::configure(const DynamicObject& config) {
        if (config.contains("smoother_enabled")) {
        	_smootherEnabled = config["smoother_enabled"];
        	_volumeToBioGrowth->setSmoothing(_smootherEnabled);
    	}
        if (config.contains("debugging_enabled")) {
        	_debuggingEnabled = config["debugging_enabled"];
    	}
        if (config.contains("debugging_output_path")) {
        	_debuggingOutputPath = config["debugging_output_path"].convert<std::string>();
    	}	}
```

In the example we are enabling the “debugging_enabled” flag by setting it to “true”. Hence we can enable settings such as smoother_enabled by setting ”smoother_enabled”: true in the config files.


### BuildLandUnit Module

A key module for all FLINT implementations is the Build Land Unit Module (BLUM). The BLUM defines the rules of developing the sequence of events and processes using both spatial data as well as database data. The rules within BLUM are configured based on the input data logic, and are restricted through the code to the key design decisions of FLINT custom implementation (e.g. SLEEK or GCBM). In simple terms, the BLUM can be considered as a series of ‘if, then’ statements. For example, if land cover changes from X to Z, then run Y events, or if tree status equals true, then apply the Tree Growth module. As with most modules, the BLUM consists of multiple modules, each completing a specific computational process.  



### SystemSettings

It is a data structure that contains variables or objects(collections) to aggregate information which can be sent to and shared amongst modules. It is a data structure that has a few system bits of information in it (see Chapman Richards example). In this way, one class of data that is loaded once in the SystemSettings can be passed to all the objects and each module can use it as it wants. But it means every time you change module settings or you want to interact, you don't have to go and edit lots of places. You've got one class or one data structure that has all your information in it.






### Disturbance Events
#### What are Disturbance Events?

Disturbance events refer to natural and human induced events like deforestation, forest fires etc. These events can have three impacts on an ecosystem that the FLINT needs to be able to address.
Move stocks from one pool to one or more other pools
Change the subsequent fluxes of pools over time
Change the ecosystem type

#### Theory and application of disturbance events

The simplest way to think of a disturbance event

| Tree AGB | Tree BGB | Dead material | Litter | Soil | Products |
| Tree AGB | | | | | |
| Tree BGB | | | | | |
| Dead material | | | | | |
| Litter | | | | | |
| Soil | | | | | |
| Products | | | | | |


### Example disturbance matrix


> TODO: // to be written
> //currently reading Ag. soil module and Chapman Richards source code for disturbances


### Disturbances in FLINT

During a FLINT run, the Sequencer handles the time with timesteps with an event queue. Disturbance events break the normal sequence of events in the event queue.  

Examples:
* If an event occurs every week, then the particular time step (month)  is divided into 4 parts.
* Assume that a set of events occur exactly the same, and are two seconds apart. Then the first event will break the time step, then two seconds will pass and after that the second event will occur, two seconds will pass, the third event will occur, and so on. Hence the time step gets broken up into as many parts as it's required to give all the events the time to occur and they do their own carbon pool moves.

### System Providers
#### SQLite Provider
Suppose you have this provider:

```json
{
   "Providers": {
       "Database": {
           "path": "..\\input_database\\gcbm_input.db",
           "type": "SQLite"
       },
[...]
```

The simplest type of query you can do would look something like this, in your "Variables" config:

```json
       "disturbance_type_codes": {
           "transform": {
               "queryString": "SELECT dt.name AS disturbance_type, dt.code AS disturbance_type_code FROM disturbance_type dt",
               "type": "SQLQueryTransform",
               "library": "internal.flint",
               "provider": "Database"
           }
       },
```

Where "provider": "Database" matches the SQLite provider name: "Database": {

You'd access that from a module the same way you would any other variable:

```c++
auto distTypeCodes = _landUnitData->getVariable("disturbance_type_codes");
const auto& distTypeCodesValue = distTypeCodes->value();
if (distTypeCodesValue.isVector()) {
	for (const auto& code : distTypeCodesValue.extract<const std::vector<DynamicObject>>()) {
		std::string distType = code["disturbance_type"];
		int distTypeCode = code["disturbance_type_code"];
	}
} else {
	std::string distType = distTypeCodesValue["disturbance_type"];
	int distTypeCode = distTypeCodesValue["disturbance_type_code"];
}
```

Where the first line gets a pointer to the IVariable, the second line (->value()) actually causes the query to execute, and the rest of the lines handle the possible results from the query - multiple rows come back as a std::vector<DynamicObject>, or if there's only a single result, it comes back as a single DynamicObject (not a vector)

SQL can be inline in the JSON config using "queryString": "select * from whatever" or stored in a separate file using "queryFile": "some/query.sql"

You can do some basic format-string type stuff in the SQL:
"SELECT * FROM species_parameters WHERE species_name LIKE {var:species}" <-- replaces {var:species} with the value of the variable named species
"SELECT growth_modifier FROM density_parameters WHERE hardwood_merchantable_carbon >= {pool:HWMerchC}" <-- replaces {pool:HWMerchC} with the value of the HWMerchC pool
"SELECT is_desert FROM climate_types WHERE precipitation <= {var:climate.mean_annual_precipitation}" <-- replaces {var:climate.mean_annual_precipitation} with the value of the mean_annual_precipitation property of the variable named climate

### Key Components of the FLINT

> TODO: Core Framework Layout

> TODO: Simple Diagram of Components

Key components of the FLINT system are:
* Land Units
 	* A Simulation Unit is a unit for which a module is applied. A Simulation Unit can represent a spatial area, such as a pixel or forest stand, or it can represent an emissions source, such as livestock. Where the Simulation Unit refers to a geographically referenced area, it is known as a Land Unit.
	* Within the databases and data-layers underpinning the FLINT, there are attributed values that describe the characteristics of each Simulation Unit. For example, for a Land Unit there may be information on the unit’s area, land type, age of vegetation, species and carbon pools. The overall framework of FLINT manages the processing of Simulation Units over time. While Simulation Units are the basis of all simulations run in FLINT, they are rarely used for reporting purposes (see Local Domain).

* Local Domain
	* Sitting above Simulation Units, is the Local Domain. The Local Domain is the collective of Simulation Units that are bound for a specific purpose. This can be, for example, to report on the changes in carbon stocks for a particular region.
	* Within FLINT, Local Domains have three main functions. Firstly, they are used to ‘house’ the variable values for all the simulation units which they represent. Secondly, through a local domain controller they assign these values to the simulation units during a simulation. Thirdly, they receive the output simulation units, and up-date the domain characteristics.

* Local Domain Controller (LDC - LocalDomainControllerBase)
	* Handles the current iteration of the Land Unit’s intended for simulation.
	* Including calling the configured Sequencer
	* Types of the LDC:
		* System Spatial LDC: SpatialTiledLocalDomainController
		* System Point LDC: LocalDomainControllerBase
	* The moja CLI program will use the config reader to instantiate the correct LDC according to type of simulation (point or spatial) as specified in the config files.
	* Built into the moja CLI program
	* Users can run default moja.cli.mulliongroup (.exe on windows) or create their own. By default it will use the inbuilt LDC in point/spatial mode and is fixed in how it decides to run the Land Units across the spatial area of interest. Users can write their own LDC implementation to extend or modify the existing CLI.
	* For example, we have added the args -tile, -block, -cell. These args let us specify which tile/block/cell to run in a spatial simulation. The T/B/C indexes represent a lat/Lon in a simple tiles referencing system.

* Land Unit Controller (&landUnitController)
	* Each thread gets its own LandUnitController which provides access to pools/variables and carbon transfers for the current pixel.

* Land Unit Data (_landUnitData)
	* This is the module’s view of the LandUnitController - it interacts with the land unit controller on behalf of the module to create and submit carbon transfers (createStockOperation/createProportionalOperation/submitOperation) and get references to pools and variables.
		* getPool()
			Gets a reference to a pool - usually a carbon pool, but a pool is really just a container for a number representing a quantity of something. Most of the work of a module is just transferring amounts between pools. When a module gets a reference to a pool, it is guaranteed by the framework to always point to the current pixel.

			For example

```c++
	_soil = _landUnitData->getPool("soil");
	_atmosphere = _landUnitData->getPool("atmosphere");
```

		* getVariable()
			Gets a reference to a variable - variables come in two main categories: transforms, which retrieve read-only data, and read/write variables that contain more “state”-type data. Note that read/write variable values persists across pixels: for example, if a variable “a” starts at 0, and a module sets it to 1 after processing a pixel, the variable’s value will still be 1 after moving on to the next pixel. Pool values are different in that they get reset to their starting/default value after every pixel.

			For example

```c++
forest_age_ = _landUnitData->getVariable("forest_age");
forest_type_ = _landUnitData->getVariable("forest_type");
```

* Notification Center (NotificationCenter)
	* It is used to control FLINT System events. It is responsible for sending signals to all the current processes in the simulation.
	* It is a data member of the Local Domain Controller
	* Events are fired using the method “postNotification” and a System Event Type (see signals.h). Modules can be configured to listen to these events (signals) and ‘react’ by running a particular function at that particular time in the simulation.

* Sequencer (SequencerModuleBase)
	* Called by the LDC, handles iterations (other than location, i.e. date) for each Land Unit.
	* Normally handles time with the timesteps (i.e. Yearly, Monthly, Daily, etc)
	* Each Sequence will fire predefined FLINT System Events (InitTiming, TimingStep, etc). These events can be subscribed to by the Modules.
	* For Disturbance event handling see: CalendarAndEventFlintDataSequencer
		* Allows events to be defined and processed mid step.
		* Any step with an Event will be broken into 2 or more segments, with Pool moves being proportioned appropriately.
		> TODO: // this part to be reviewed
		> TODO: [Insert Diagram Perhaps]
		* See chapman richards example of event handling
		* In Build Land Unit module, populating the event queue

* Providers (IProviderInterface)
	* Defined datasets that allow the FLINT to access input data sources such as Relational Databases, Spatial Data, etc.
	* Defined interface allows location lookup of data when running spatially - Lat & Lon.
	* Various base types of Providers exist to define interfaces available:
		* IProviderNoSQLInterface
		* IProviderRelationalInterface
		* IProviderSpatialRasterStackInterface
		* IProviderSpatialRasterInterface
		* IProviderSpatialVectorInterface

* Datarepository (moja.datarepository)
	>	// from Max
	* It contains various types of data providers that read from spatial layers, databases, etc. and make that data available through the various transform classes, and finally through named variables that modules read from. One example would be an ExternalVariable with a LocationIdxFromFlintDataTransform using a TileRasterReaderGDAL provider to read a data value for the current pixel from a spatial layer.

```c++
void ForestPlantEvent::configure(DynamicObject config, const flint::ILandUnitController& landUnitController,  datarepository::DataRepository& dataRepository)
{
   DisturbanceEventBase::configure(config, landUnitController, dataRepository);
   forest_type_id = config["forest_type_id"];
}
```

* Operation Manager (IOperationManager)
	* This component manages operations created on Pools. Designed to enable Mass Balance and allows fluxes to be timed according to Steps and User preference (i.e. immediately or the end of each timing step). See System Modules:
		* TransactionManagerEndOfStepModule
		* TransactionManagerAfterSubmitModule
	* These modules make calls on the Operations Manager to apply registered Operations, but at different times. FullCAM for example will process all operations at the end of each step, GCBM will do them immediately after being submitted.
	* The timing change has the following effects:
		> TODO: Might need to get Max and malcolm.francis@mulliongroup.com to add comments here

	> TODO: // From Max, need to review or summarise/reword this
		* Different sets of science modules have different needs in terms of when the carbon transfers get applied because it affects the view that the modules have of the carbon pools. GCBM is a long chain of mostly sequential modules, so we use TransactionManagerAfterSubmitModule which applies the pool operations after every single event the system fires (actually we're a little bit paranoid about this, so we often manually apply them in the science modules too) - we want to ensure that all of our modules are looking at the most current pool values, because some of our modules use them to inform the next movements of carbon. Other sets of modules might be more simultaneous - that is, they set up a bunch of proportional operations based on the pool values at the beginning of the timestep, then they all get applied at once. It's also possible to manage this entirely in the science modules - these are really just helpers.
		* Long story short: TransationManagerAfterSubmitModule is in there mainly for GCBM, pretty much everything else should be using TransactionManagerEndOfStepModule.
		* Now that I look at this stuff again, GCBM would probably blow up if line 20 of AfterSubmitModule ever got uncommented.
	* Different implementations can be created. Using different methods to make the movements between Pools. For example, using Matrix tools (i.e. EIGEN) or a simple list method (OperationManagerSimple).
	* addTransfer
		* After creating an operation (proportional or absolute), which tells the FLINT that the associated transfers are either proportions or specific amounts, you need to add one or more transfers to the operation before submitting it. A transfer is a movement of units (carbon or whatever the pool represents) from one pool to another. For example: _landUnitData->createProportionalOperation(), then addTransfer(softwood, deadwood, 0.5) would transfer half of the amount in the softwood pool to the deadwood pool.
 		* Transfers within the same operation occur simultaneously - that is, if the same operation had two transfers instead:
			* For example: softwood -> deadwood @ 0.5, softwood -> soil @ 0.5, half of the softwood would go to deadwood and half would go to soil and none would remain.
	* Submit Operation function
		* Normally used to execute operations after defining them. Submits an operation (set of pool transfers) to the system. All transfers are tracked by the system so that an output module can read them and write the transfer information to a database or other repository.

		For example:

```c++
submitOperation(operation);
```

		* Operation types are:
			* Stock  createStockOperation()
				- move explicit amounts between specified Pools
					For example (Here EF_1_value  stands for Emission Factor)

```c++
auto operation = _landUnitData->createStockOperation();
   operation->addTransfer(soil_, atmosphere_, (fert.quantity * EF_1_value) / fert.runtime);
   _landUnitData->submitOperation(operation);
```

			* Proportional createProportionalOperation()
 				- move a proportion between specified Pools
					For example, here carbon moves from _pool1 to _pool2  in ratio of ratio_1, then carbon from _pool2 moves to _pool3 in ratio_2 and so on

```c++
void TestModule::onTimingStep() {
   auto operation = _landUnitData->createProportionalOperation();
   operation->addTransfer(_pool1, _pool2, ratio_1)
       ->addTransfer(_pool2, _pool3, ratio_2)
       ->addTransfer(_pool3, _pool1, ratio_3);
   _landUnitData->submitOperation(operation);
}
```

* Modules (ModuleBase)
	* Both internal FLINT and custom modules
	* Attached to a 0 or Many FLINT Events that are fired by the LDC or Sequencer
	* Can interact with Pools and Variables and use Provider Data

* Common Data: Pools (IPool)
	* Single set of Pool values shared across all Land Units
	*	Can be reset at various stages
	* Can be Viewed and modified at various time - used to do Pool reporting
	* Defined interface in interactions - allows mass balance to be maintained
	* Pools can report more than Carbon - simply a bucket with a Floating Point number, that modules can make moves to and from

* Common Data: Variables (IVariable)
		* Used by the Modules to store data they may need to share with the implementation (other Modules).
		* For example: Boolean: TreeExists would be a boolean to tell other Modules that a tree has been planted.
		* Variables can also be moja::flint::ITransform or moja::flint::IFlintData
		* Transforms allow the system to replace a variable value with a value returned by the Transform - hence calling code that can make calculations and access current Pool, Variable and Provider values.
		* For example: accessing a Spatial Layer from a Provider would automatically get the data for the current Lat & Lon

		For example, these variables are declared in header file of the module

```c++
	 flint::IVariable* forest_exists_;
   flint::IVariable* forest_age_;
   flint::IVariable* forest_type_;
```

		* Then in the module CPP file they are used in this way

```c++
   forest_exists_ = _landUnitData->getVariable("forest_exists");
   forest_age_ = _landUnitData->getVariable("forest_age");
   forest_type_ = _landUnitData->getVariable("forest_type");
```

		* Hence data from _landUnitData can be stored in a variable of class IVariable

* Common Data: Variables (IFlintData)
	* FlintData allows for a more complex data structure that can be shared across Modules. Giving more that just the single value() returned by a Transform.
	* With Variables and Transforms we use the method ->value() ? allowing use to substitute either
	* IFlintData doesn't have this method, so allows more complex object use - if you know what type you're looking for
	* Users can derive a C++ object from the FlintData and customise it for various purposes (for eg. importing and storing GIS data in memory).
 	* IFlintData also allows unpacking from JSON - so can be completely defined in JSON config. An example of this is the EventQueue stuff with CalendarAndEventFlintDataSequencer

FLINT.example: `FLINT.Example/Run_Env/config/point_forest_config.json` has

```json
{
"eventqueue": {
"flintdata": {
"library": "internal.flint",
"type": "EventQueue",
"settings": {
"events": [
{
"date": {
"$date": "2001/01/01"
},
"id": 1,
"type": "chapman_richards.ForestPlantEvent",
"name": "Plant Dryland Forests",
"forest_type_id": 1,
"age": 0.0
},
{
"date": {
"$date": "2050/01/01"
},
"id": 1,
"type": "chapman_richards.ForestClearEvent",
"name": "Clear Dryland Forests"
}
]}}}},
```

Not completely accurate, but an indication of how the system is trying to process

## Questions
How to get started with disturbance events modules?
Could you explain what this code exactly does? Line 46 at https://github.com/moja-global/FLINT.Example/blob/master/Source/moja.modules.chapman_richards/include/moja/modules/chapman_richards/disturbanceevents.h


```c++
typedef std::vector<std::shared_ptr<DisturbanceEventBase>>::value_type value_type;
typedef std::vector<std::shared_ptr<DisturbanceEventBase>>::iterator iterator;
typedef std::vector<std::shared_ptr<DisturbanceEventBase>>::const_iterator const_iterator;
typedef std::vector<std::shared_ptr<DisturbanceEventBase>>::size_type size_type;

```



//Need help in understanding this
FLINT.Example/disturbanceeventmodule.cpp
Especially Line 52 and how does this all connect and fire disturbances.

```c++
   const auto forest_types = std::static_pointer_cast<const ForestTypeList>(
       forest_types_var->value().extract<std::shared_ptr<flint::IFlintData>>());
```

Questions to ask: //answers to be merged with the documentation
What is?/ Please explain:
* _landUnitData
	* This is the module’s view of the LandUnitController - it interacts with the land unit controller on behalf of the module to create and submit carbon transfers (createStockOperation/createProportionalOperation/submitOperation) and get references to pools and variables.

* landUnitController
	* Each thread gets its own LandUnitController which provides access to pools/variables and carbon transfers for the current pixel.

* Signals
	* Modules in the FLINT are event-driven, meaning that to do any work, they need to choose one or more events to subscribe to and provide a handler method for. The sequence of events is found in a couple of different places: the LocalDomainController (i.e. moja.flint.SpatialTiledLocalDomainController), and the sequencer module (i.e. moja.flint.CalendarAndEventSequencer) - see all the calls to _notificationCenter.postNotification. In general, the LocalDomainController deals with system and block-level events, and the Sequencer module deals with the pixel-level time loop - the events fired from there are typically where most of the module work is done.

* onLocalDomainInit() // why is it used
	* This event is fired once per thread (but each thread has its own instance of the module) at the start of the simulation - modules typically subscribe to this event in order to store references to pools and variables using _landUnitData->getPool and _landUnitData->getVariable. At this stage, pools and variables have been initialized, so if a module needs to pre-load some data from a database, for example, it can do that here. There is no “current pixel” during this event, so spatial data is not available.

* LocalDomain
	Basically the current thread and its own set of module instances, etc.

* getPool
	* Gets a reference to a pool - usually a carbon pool, but a pool is really just a container for a number representing a quantity of something. Most of the work of a module is just transferring amounts between pools. When a module gets a reference to a pool, it is guaranteed by the framework to always point to the current pixel.

* getVariable
	* Gets a reference to a variable - variables come in two main categories: transforms, which retrieve read-only data, and read/write variables that contain more “state”-type data. Note that read/write variable values persists across pixels: for example, if a variable “a” starts at 0, and a module sets it to 1 after processing a pixel, the variable’s value will still be 1 after moving on to the next pixel. Pool values are different in that they get reset to their starting/default value after every pixel.

* addTransfer
	* After creating an operation (proportional or absolute), which tells the FLINT that the associated transfers are either proportions or specific amounts, you need to add one or more transfers to the operation before submitting it. A transfer is a movement of units (carbon or whatever the pool represents) from one pool to another. For example: _landUnitData->createProportionalOperation(), then addTransfer(softwood, deadwood, 0.5) would transfer half of the amount in the softwood pool to the deadwood pool.
	* Transfers within the same operation occur simultaneously - that is, if the same operation had two transfers instead: softwood -> deadwood @ 0.5, softwood -> soil @ 0.5, half of the softwood would go to deadwood and half would go to soil and none would remain.

* submitOperation
	* Submits an operation (set of pool transfers) to the system. All transfers are tracked by the system so that an output module can read them and write the transfer information to a database or other repository.

* Datarepository
	* Moja.datarepository contains various types of data providers that read from spatial layers, databases, etc. and make that data available through the various transform classes, and finally through named variables that modules read from. One example would be an ExternalVariable with a LocationIdxFromFlintDataTransform using a TileRasterReaderGDAL provider to read a data value for the current pixel from a spatial layer.
	* The _modules.base_exports.h file in header files/other. This file is present in every module. Why is it used and how to set it up while making a new module. //see page 8
	* This file is generated by CMake and, when CMake is properly configured for a new module, defines the <some module>_API macro that needs to go in front of any classes you want to make visible to the FLINT, i.e. class KOREA_API KoreaGrowthCurveTransform : public flint::ITransform. Without the KOREA_API macro in front, the FLINT won’t be able to use that class from the module dll.

* Please explain disturbance events in a module, both in science and its implementation in code in detail.
	* There are many functions defined in imodule.h header like StartupModule() , ShutdownModule() and event message functions like onLocalDomainInit(), onTimingInit() etc. I think documenting the use of these functions will be very helpful as developers can customise their simulations very easily with them. Even the RothC module uses functions like onTimingInit() and onTimingStep().
	* Check page 9 for an example.

* How do we use them?
	* I also want to know about LocalDomain and how it is used in the config files.

* What are operations and different kinds of operations? How to use the operation functions in the modules (with respect to the code)?

* Why do we use FlintDataRegistration, FlintDataFactoryRegistration, DataRepositoryProviderRegistration in LibraryFactory?//solved on slack

* How do we make a module runnable from the current stage of the documentation?

* Is the BuildLandUnit module necessary for every spatial model? If yes, then how can make a template out of it (most important classes and methods or something which may help new users to start developing their own spatial models)?

* How to use the SQLwriter to run SQL commands during a module run?

* Could you explain Line 79 onwards? Also why is this part not there in the Soil module buildlandunitmodule.cpp? https://github.com/moja-global/FLINT.Example/blob/82398c56a19b02ba95580352c076de8af1b40ad4/Source/moja.modules.chapman_richards/src/buildlandunitmodule.cpp#L79

* What is meant by systemSettings in   
explicit BuildLandUnitModule(SystemSettings& systemSettings) ? https://github.com/moja-global/FLINT.Example/blob/82398c56a19b02ba95580352c076de8af1b40ad4/Source/moja.modules.chapman_richards/include/moja/modules/chapman_richards/buildlandunitmodule.h#L26

* How to add a variable to the config file and the module and the ability to read it’s value and use it in the module?

* How do we control the randomness of a disturbance event (probability of happening)?
