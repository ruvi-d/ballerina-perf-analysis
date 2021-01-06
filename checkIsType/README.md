# checkIsType performance analysis

This analyses the performance of the runtime type checking. This is related to the use of the `is-expr` that tests where a value belongs to a type. `is-expr` has already been identified as a performance bottleneck and this analysis tries to profile the issues possibly identify a low cost solution.

## Executive summary
* It is apparent that the comparison of similar type Objects contribute the most to the execution time
* Propose to implement a simple caching mechanism to store Object type checking results

## Analysis
[Detailed analysis](checkIsType_analysis.ipynb)

## How to recreate the results
* Add [this patch](https://github.com/ruvi-d/ballerina-lang/commit/97722b9128e1c254237f8ae135c1ffa62030aa19) to `ballerina-lang` repo and build the `ballerina-rt` with the command `./gradlew ballerina-rt:build -x check`
* Copy the new `ballerina-rt*.jar` to a compatible Ballerina pack. e.g `cp ./bvm/ballerina-rt/build/libs/ballerina-rt-* $BALLERINA_HOME/distributions/ballerina-slp8/bre/lib/`
* Clone `https://github.com/ballerina-platform/module-ballerina-http`
* Navigate to the `http-ballerina-tests` folder and run `ballerina test`
* This will generate a `typecheck_calls.csv` file with the data
* The CSV can be processed and visualized with the [Jupyter notebook](checkIsType_analysis.ipynb)) here
  * You will need to install Python and [Jupyter](https://jupyter.org/install)
  * You can open the notebook via browser or VS code