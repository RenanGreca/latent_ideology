# Latent ideology method
Correspondence Analysis applied for calculating 'ideology scores' given an adjacency matrix.

Method for applying the correspondence analysis method for the purpose of calculating 
  an 'ideology score' as stated in [1][2].
  
  -  [1] J. Flamino, A. Galezzi, S. Feldman, M. W. Macy, B. Cross, Z. Zhou, M. Sera
  no, A. Bovet, H. A. Makse, and B. K. Szymanski,
  'Shifting polarization and twitter news influencers between two us presidential elections', 
  arXiv preprint arXiv:2111.02505 (2021).
  
  
  -  [2] Max Falkenberg, Alessandro Galeazzi, Maddalena Torricelli, Niccolo Di Marco, Francesca Larosa, Madalina Sas, Amin Mekacher, 
  Warren Pearce, Fabiana Zollo, Walter Quattrociocchi, Andrea Baronchelli,
  'Growing polarisation around climate change on social media',
  https://doi.org/10.48550/arXiv.2112.12137 (2021).


## How to use

It's simple, just 

```
pip install latent-ideology
```

There are a couple of things you can do here. You can:

-  Make an adjacency matrix given a 'connection dataframe' between targets and sources. (see more in the examples folder)
-  Given an adjacency matrix, calculate scores in 1 (or more) dimensions. 
-  Given a 'connection dataframe', you can simple apply the function 'apply_method' and this will generate 2 dataframes: one for the score of the targets and one for the scores of the sources. 
-  Altenatively, you can apply a simplified method by using the function 'apply_simplified_method'. Here, the source's scores are calculating by transposing the adjacency matrix and making reduction of dimensionality following the proccedures of [2].. instead of considering the score of the sources as the mean of the scores of the targets that had interacted with the source. 

## Computing time

Cosiderations: 

**(please read the file latent_ideology_class.py for insights)**

-   The more you relax $n (n>2)$, the restricted it gets and the faster it runs. Simple. 
-   Since $m$ is explicitly the number of columns of the matrix to further compute the method, the more restricted you get ($m$ small), the faster it runs.

For comparison:

I've run the package for a big connection dataset (1.5mill rows) with n=10 and m=300. This gave a (25000, 300) adjacency matrix and the final run time was around 35 sec. 

## Updates

### v0.0.3 (7/30/2022)

Added some functionalities:

-   If your input dataframe has a 'weight' column indicating the weight of the interaction between targets-sources, you can now specify it to save some running time.

-   You can now create an alternate dataframe indicating to which sources each target had interacted with. 

And some changes: 

-   Since we want a fixed number of sources, I switch the order of the filtering: the program first filter out targets that don't pass the minimum n-distinct sources to interact with, and then it consider the m-top sources threshold. This way, you can make sure that m is a fixed number. The other way around, you could end up with less sources than expected due to applying the the 'n-filter' to a reduced dataset. 

### v0.0.4 (8/24/2022)

Added:

-    New parameter to play with. k let you put a threshold to the maximum interactions considered between target-sources. This sometimes accelerates the formation of non-unimodal distributions. Essential when studying polarization. In practice, k let you filter out outlier or non representatives targets. 

-    'Total interaction' column added when building the adjacency matrix and setting ```detailed_target_list = True```

Fixed:

-    Wrong filtering when applying the '$n$ distinct sources to interact with' threshold. It only worked when setting detailed_target_list = True. 

-    Added '.copy()' to the original input dataframe in the main functions. Original DataFrames were forced to change when renaming columns. Not anymore! (sorry 'bout that) 

---
  
Made with love by **Fede Moss**
