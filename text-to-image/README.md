# Text to Image \ Cropping

I printed [quotes](text-to-image/quotes.yaml) onto [images](imgs) using [quimg.py](text-to-image/quimg.py).

[.github/workflows/quimg.yml](/.github/workflows/quimg.yml) sends an image quote status update every 9 hours at the 11th minute.

```yaml
    - cron:  '33 15 * * *'
    - cron:  '11 23 * * *'
```


## Cropping Tools

Aspect Ratio Calculator:

https://andrew.hedges.name/experiments/aspect_ratio/

Need Imagemagick for all this stuff:

https://imagemagick.org/index.php

### crop all in directory

http://www.fmwconcepts.com/imagemagick/aspectcrop/index.php

```bash
for f in * ;
./aspectcrop -a 1920:1080 $f cropped.$f
```

### Using Convert 
Aspectcrop only crops proportionally, if you want exact dimensions cropped, use convert.

```bash
for f in * ;   
convert $f -crop 1080x1080+0+0 -gravity center insta.$f
```

## Optimization
### Reduce size of jpg in place

https://stackoverflow.com/questions/50985087/how-to-reduce-the-file-size-on-jpeg-images-in-batch-mac

```bash
mogrify -define jpeg:extent=300kb *.jpg
```

### PngQuant

https://pngquant.org/

