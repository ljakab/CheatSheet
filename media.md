FFmpeg tricks
=============

Input
-----

  * Time-lapse video from still photos:

    -f image2 -pattern_type glob -i 'DSC_*.jpg'

  * Grab screen:

    -f x11grab -r 60 -s 1920x1080 -i :0.0

Output
------
  * Copy the audio stream unchanged:

    -c:a copy

  * Set the size:

    -s 1920x1080

  * Encode to ProRes 422 (Set Vendor ID to ap10 so QuickTime and FCP won't
    complain. Profiles: Proxy (0), LT (1), SQ (2) and HQ (3). Qscale is from 0
    to 32. A qscale of 9 - 13 gives a good result without exploding the space
    needed too much. 11 would be a good bet, 9 if a slightly better quality is
    required. When space is not a problem, go with qscale 5 or less.

    -c:v prores_ks -profile:v 2 -qscale:v 11 -vendor ap10 -pix_fmt yuv422p10le output.mov
