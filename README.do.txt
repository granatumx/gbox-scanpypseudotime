!bquote
`gbox-scanpypseudotime` is a gbox that provides a number of gboxes from Python. It is part of the standard gbox set.
!equote

===== Prerequisites =====

You mainly need a working copy of "Docker": "http://docker.com". It is used
exclusively to manage system configurations for running numerous tools
across numerous platforms.

===== Installation =====

* All docker images are at "https://hub.docker.com/u/granatumx".
* All github repos are at "https://github.com/granatumx/*".

First set up your scripts and aliases to make things easier. This command should pull the container if
it does not exist locally which facilitates installing on a server.
!bc sys
source <( docker run --rm -it granatumx/scripts:1.0.0 gx.sh )
!ec

This command makes `gx` available. You can simply run `gx` to obtain a list of scripts available.

The GranatumX database should be running to install gboxes. The gbox sources do not need to be installed.
A gbox has a gbox.tgz compressed tar file in the root directory which the installer copies out and uses
to deposit the preferences on the database. Since these gboxes are in fact docker images, they will be
pulled if they do not exist locally on the system. Convenience scripts are provided for installing specific gboxes.

!bc sys
$ gx run.sh                                  # Will start the database, taskrunner, and webapp
$ gx installGbox.sh granatumx/gbox-scanpypseudotime:1.0.0  # Install this gbox

# Now go to http://localhost:34567 and see this gbox installed when you add a step.
!ec

===== Notes =====

DPT is the abbreviation of “diffusion pseudotime”, where “d” represents “diffusion”. This method was proposed in the publication “Diffusion pseudotime robustly reconstructs lineage branching” (PMID 27571553). It measures transitions between cells using diffusion-like random walks, and makes it possible to reconstruct the developmental progression of cells and identify transient or metastable states, branching decisions and differentiation endpoints.

As to the meaning of the output plots, their DC1 and DC2 axes indicate the value of the first 2 diffusion components. They are the top 2 largest eigenvalues of the transition matrix, which approximates the dynamic transitions of cells through stages of the differentiation process. This transition matrix is computed using a nearest neighbor graph whose edge weights have a Gaussian distribution with respect to Euclidean distance in gene expression space and transition probabilities of this matrix correspond to the edge weights. DC1 and DC2 are used here for visualizing the single cells in the 2-dimensional plots. 

The difference between the 2 output plots is about their color keys. The first one has a continuous color key, while the second one has a discrete color key, both of which are used to show the differentiation stages of the cells, or time points of the random walk process. If a cell in the first plot has a color corresponding to a small value in the continuous color key, it means this cell differentiates early, while one with a color corresponding to a large value in the color key means it differentiates late. As to the second plot, the continuous time value is discretized and the cells are correspondingly divided into different groups, according to their discrete color values. Hence, cells in the group with a small color value constitute a group differentiated early, while other groups differentiate late.
