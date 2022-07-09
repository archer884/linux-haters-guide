# How to clip a video with ffmpeg

ffmpeg -ss 01:21:46 -i <input_filename> -c copy -t 3002 <output_filename>

Both the ss and t params can be provided either as a number of seconds or as a zero-padded duration of form HH:MM:SS. Note that length T is relative to timestamp SS and is NOT a timestamp.
