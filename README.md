# slideslive-download


1) Install https://github.com/ytdl-org/youtube-dl/
2) Install https://github.com/PeterTheOne/slideslive-slides-dl (make sure to fix versioning if you'd like to download EMNLP'20 or later, the links should be like https://d2ygwrecguqg66.cloudfront.net/data/presentations/id/v3/id.xml)

For EMNLP'20 or later you'll need to make fixes in slideslive-slides-dl.py  like
```
file_path = '{0}/v3/{1}.xml'.format(folder_name, video_id)
    if not os.path.exists(file_path):
        xml_url = '{0}{1}/v3/{1}.xml'.format(base_xml_url, video_id)
```
ACL'20: https://d2ygwrecguqg66.cloudfront.net/data/presentations/38929870/38929870.xml (XML file containing timestamps for slides)
EMNLP'20: https://d2ygwrecguqg66.cloudfront.net/data/presentations/38939785/v3/38939785.xml

3) Download the video, e.g. youtube-dl https://slideslive.com/38929870/
4) Get slides and their timestamps: 
```
cd slideslive-slides-dl
``` 
and run 
```
python3 slideslive-slides-dl.py https://slideslive.com/38929870/
```
It will create a folder like "38929870-" that contains ffmpeg_concat.txt  
5) Make a slideshow (set the scale so it fits the video size as well): 
```
 ffmpeg -f concat -i 38929870-/ffmpeg_concat.txt -vsync vfr -pix_fmt yuv420p -filter:v scale=1364:1080 slides-out.mp4
```
8) Merge the video with the slideshow, e.g.
```
ffmpeg -i ../SIGMORPHON\ 2020\ Shared\ Task\ 0\ -\ Typologically\ Diverse\ Morphological\ Inflection-434566251.mp4 -i slides-out.mp4 -filter_complex hstack output.mp4
```
