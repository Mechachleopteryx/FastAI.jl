# FastAI

[Documentation](https://FluxML.github.io/FastAI.jl/dev)

FastAI.jl is inspired by [fastai](https://github.com/fastai/fastai), and is a repository of best practices for deep learning in Julia. Its goal is to easily enable creating state-of-the-art models. FastAI enables the design, training, and delivery of deep learning models that compete with the best in class, using few lines of code.

Install with

```julia
using Pkg
Pkg.add("FastAI")
```

or try it out with this [Google Colab template](https://colab.research.google.com/gist/lorenzoh/2fdc91f9e42a15e633861c640c68e5e8).


## Example

Training an image classification model:

```julia
using FastAI
path = datasetpath("imagenette2-160")
data = Datasets.loadfolderdata(
    path,
    filterfn=isimagefile,
    loadfn=(loadfile, parentname))
classes = unique(eachobs(data[2]))
method = BlockMethod(
    (Image{2}(), Label(classes)),
    (
        ProjectiveTransforms((128, 128), augmentations=augs_projection()),
        ImagePreprocessing(),
        OneHot()
    )
)
learner = methodlearner(method, data, Models.xresnet18(), ToGPU(), Metrics(accuracy))
fitonecycle!(learner, 10)
```

Please read [the documentation](https://fluxml.github.io/FastAI.jl/dev) for more information and see the [setup instructions](docs/setup.md).
