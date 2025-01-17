# This is a PHP extension builder for libpuzzle.

Install php-dev and libpuzzle from your package manager (`apt install php-dev libpuzzle-dev` for example)

Here are the basic steps in order to install the extension:

```
cd src
phpize
./configure --with-libpuzzle
make clean
make
make install
```

If libpuzzle is installed in a non-standard location, use:
./configure --with-libpuzzle=/base/directory/for/libpuzzle

Then edit your php.ini file and add:

`extension=libpuzzle.so`


# Usage

The PHP extension provides bindings for the following tuning functions:
- puzzle_set_max_width()
- puzzle_set_max_height()
- puzzle_set_lambdas()
- puzzle_set_noise_cutoff()
- puzzle_set_p_ratio()
- puzzle_set_contrast_barrier_for_cropping()
- puzzle_set_max_cropping_ratio()
- puzzle_set_autocrop()

Have a look at the puzzle_set man page for more info about those.

Getting the signature of a picture is as simple as:

`$signature = puzzle_fill_cvec_from_file($filename);`

In order to compute the similarity between two pictures using their
signatures, use:

`$d = puzzle_vector_normalized_distance($signature1, $signature2);`

The result is between 0.0 and 1.0, with 0.6 being a good threshold to detect
visually similar pictures.

The PUZZLE_CVEC_SIMILARITY_THRESHOLD, PUZZLE_CVEC_SIMILARITY_HIGH_THRESHOLD,
PUZZLE_CVEC_SIMILARITY_LOW_THRESHOLD and PUZZLE_CVEC_SIMILARITY_LOWER_THRESHOLD
constants can also be used to get common thresholds :

```
if ($d < PUZZLE_CVEC_SIMILARITY_THRESHOLD) {
  echo "Pictures look similar\n";
}
```

Before storing a signature into a database, you can compress it in order to
save some storage space:

`$compressed_signature = puzzle_compress_cvec($signature);`

Before use, those compressed signatures must be uncompressed with:

`$signature = puzzle_uncompress_cvec($compressed_signature);`

