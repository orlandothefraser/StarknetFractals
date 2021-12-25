# StarknetFractals
Generating the mandelbrot set on Starknet. Computes a 100x100 mandelbrot plot and stores necessary data to recontruct it onchain in 100 storage felts. One can then call the contract to retrieve the data and reconstruct the plot. 

## Environment Setup 

Create a python environment, activate it, and install Cairo within it as described in: https://www.cairo-lang.org/docs/quickstart.html  

Install Nile:
```bash
pip install cairo-nile
```

## Compiling and Deploying 

Compile: 
```bash
nile compile contracts/mandelbrotOnChain.cairo
``` 

Run a local starknet-devnet node:
```bash
nile node
``` 

Deploy (using an alias of your choosing):
```bash
nile deploy mandelbrotOnChain --alias mandelbrotOnChain_Instance
```  
## Generation and Retrieval 

Invoke the contract using the script supplied to generate the Mandelbrot set and store the data to produce it within the contract: 
```bash
python3.7 scripts/generateMandelbrotOnChain.py mandelbrotOnChain_Instance 
``` 

Retrieve the Mandelbrot set data from within the contract and generate the plot of it using the supplied script:
```bash
python3.7 scripts/retrieveMandelbrotOnChain.py mandelbrotOnChain_Instance 
``` 
The resulting plot is stored within the images directory. It should look like this!

![alt text](https://github.com/orlandothefraser/StarknetFractals/blob/main/images/mandelbrot_100_25.png)





Generate the plot via running the scripts/deployment.py script. This first compiles and deploys the contract before generating the plot. Output plot will be saved in the images folder.

The generation is batched into calls to the mandelbrot.cairo contract. Each batch generates a certain number of points. From tests, 100 points per call is the rough limit before rescource limits are hit. 

Here is an example 40x40 pixel plot. It took 553 seconds total to generate. 
![alt text](https://github.com/orlandothefraser/StarknetFractals/blob/main/images/mandelbrot_40_25.png)

Math for fixed point complex numbers was required here but could be used for other things, so I made a separate cairo file ComplexMath.cairo with this stuff in. 

TODO:
Currently runs on local devnet via nile. Add mainnet support
There are additionally some optimizations that can be taken to reduce computation per pixel generation. 




This is what we are aiming for - 1000x1000 plot in as few calls as possible.
![alt text](https://github.com/orlandothefraser/StarknetFractals/blob/main/images/mandelbrot_1000_25.png)



