# 2D puzzle solving using curve and texture matching

This is a general repository for information about the 2D puzzle solving task, which includes

- Contour-based matching
- Texture-based matching
- Global matching

# 2D Datasets

| Dataset | Type | Fragments cut | Fragment quantity | Erosion | Missing fragments |
|:---|:---|:---|:---|:---|:---|
| PuzzleSolving-Tool [1, 1b] | Generator from 2D image | Smooth curves | Parameter-based (1-100) | No | No |
| 2DPuzzleSolving-Tool [2, 1b] | Generator from 2D image | Variable (WIP*) | Parameter-based (1-100) | No (WIP*) | Parameter-based (0-100%)|
| DAFNE [3, 3b] | Synthetically created from 2D image | Voronoi on random or clustered input points (synthetic and unrealistic) | Variable (~50) | Yes (synthetic) | Yes (random) |
| Technion [4] | Synthetically created from 2D image | Based on dry mud (synthetic, but realistic) | Variable (~25) | Yes (mud-based) | No |
| RePAIR 2D | Rendered from 3D model | Actually broken frescoes (real) | Variable (3 - 20) | Yes (real) | Sometimes |
| RePAIR 2D [5]| RGB images captured by a smartphone| Actually broken frescoes (real) | Variable (6 - 9 pieces in 3 groups) | Yes (real) | No |

### Notes
**WIP*** = the code has been forked and extensions are possible. The shape of the fragment can be switched between smooth or hard lines, missing fragments are now supported and erosion will be added as well.

Check out the [tool](https://github.com/RePAIRProject/2DPuzzleSolving-tool) to generate some fragments.

The DAFNE dataset requires access. You can write to [daf-challenge-group@unipv.it](mailto:daf-challenge-group@unipv.it) to get access.

The RePAIR 2D dataset is in development.

## References
[1] Puzzle generation: [Github Repo for the generator](https://github.com/xmlyqing00/PuzzleSolving-tool) - [Webpage](https://xmlyqing00.github.io/PuzzleSolving-tool/)

[1b] JigsawNet: Shredded Image Reassembly Using Convolutional Neural Network and Loop-based Composition. C. Le and X. Li, IEEE Transactions on Image Processing (TIP), 28(8):4000-4015, 2019. [GitHub](https://github.com/Lecanyu/JigsawNet), [Project page](https://www.ece.lsu.edu/xinli/PuzzleSolving/index.html), [Dataset](https://www.ece.lsu.edu/xinli/FragmentImageRepository/index.html), [Dataset2](https://drive.google.com/drive/u/0/folders/1sUIcAzFTJNAAEEhqdYAKMKgzjVwRvsP4)

[2] Puzzle generation: [Our fork to extend fragment generation](https://github.com/RePAIRProject/2DPuzzleSolving-tool)

[3] Piercarlo Dondi, Luca Lombardi, Alessandra Setti (2020), [DAFNE: a dataset of fresco fragments for digital anastlylosis](https://www.sciencedirect.com/science/article/pii/S0167865520303408), Pattern Recognition Letters, 138 (2020), pp. 631-637, Elsevier, DOI:10.1016/j.patrec.2020.09.015.

[3b] DAFNE: [Challenge Page](https://vision.unipv.it/DAFchallenge/DAFNE_dataset/)

[4] Derech, Niv, Ayellet Tal, and Ilan Shimshoni. "*Solving archaeological puzzles*" Pattern Recognition 119 (2021): 108065. [Software & Data](https://cgm.technion.ac.il/Computer-Graphics-Multimedia/Software/Software.html) [Paper](https://www.sciencedirect.com/science/article/pii/S0031320321002521)

[5] 2D images of 3 groups of RePAIR fragments [Link to download](https://drive.google.com/drive/folders/11vPh2MnRfGK4Xy2kHYS1ZWNWoIlcNSoQ?usp=sharing)

