# NAME

JSON::Slurper - Convenient file slurping and spurting of data using JSON

# STATUS

<div>
    <a href="https://travis-ci.org/srchulo/JSON-Slurper"><img src="https://travis-ci.org/srchulo/JSON-Slurper.svg?branch=master"></a>
</div>

# SYNOPSIS

    use JSON::Slurper qw(slurp_json spurt_json);

    my @people = (
      {
          name => 'Ralph',
          age => 19,
          favorite_food => 'Pizza',
      },
      {
          name => 'Sally',
          age => 23,
          favorite_food => 'French Fries',
      },
    );

    spurt_json @people, 'people.json';

    my @people_from_file = slurp_json 'people.json';

    # or get as a reference
    my $people_from_file = slurp_json 'people.json';

    # Same as above with Object-Oriented interface
    my $json_slurper = JSON::Slurper->new;

    $json_slurper->spurt(\@people, 'people.json');

    my @people_from_file = $json_slurper->slurp('people.json');

    # or get as a reference
    my $people_from_file = $json_slurper->slurp('people.json');

    # ".json" is added ad the file extension if no file extension is present.
    # This saves to people.json
    spurt_json @people, 'people';

    # This reads from people.json
    my @people_from_file = slurp_json 'people';

# DESCRIPTION

JSON::Slurper is a convenient way to slurp/spurt (read/write) Perl data structures to and from JSON files. It tries to do what you mean, and allows you to provide your own JSON encoder/decoder if necessary.

# DEFAULT ENCODER

Both the ["FUNCTIONAL INTERFACE"](#functional-interface) and the ["OBJECT-ORIENTED INTERFACE"](#object-oriented-interface) use the same default encoders. You can provide your own encoder whether you use the ["FUNCTIONAL INTERFACE"](#functional-interface) or the ["OBJECT-ORIENTED INTERFACE"](#object-oriented-interface).

## Cpanel::JSON::XS

If you have the recommended [Cpanel::JSON::XS](https://metacpan.org/pod/Cpanel::JSON::XS) installed, this is the default used:

    Cpanel::JSON::XS->new
                    ->utf8
                    ->pretty
                    ->canonical
                    ->allow_nonref
                    ->allow_blessed
                    ->convert_blessed
                    ->escape_slash
                    ->stringify_infnan

## JSON::PP

If you are using [JSON::PP](https://metacpan.org/pod/JSON::PP), this is the default used:

    JSON::PP->new
            ->utf8
            ->pretty
            ->canonical
            ->allow_nonref
            ->allow_blessed
            ->convert_blessed
            ->escape_slash

# FUNCTIONAL INTERFACE

## slurp\_json

- slurp\_json $filename, \[$json\_encoder\]

    # values can be returned as refs
    my $ref = slurp_json 'ref.json';

    # or as an array or hash
    my @array = slurp_json 'array.json';

    my %hash = slurp_json 'hash.json';

    # You can pass your own JSON encoder
    my $ref = slurp_json 'ref.json', JSON::PP->new->ascii->pretty;

    # If no extension is provided, ".json" will be used.
    # Reads from "ref.json";
    my $ref = slurp_json 'ref';

This reads in JSON from a file and returns it as a Perl data structure (a reference, an array, or a hash). You can pass in your own JSON encoder/decoder as an optional argument, as long as it is blessed
and has `encode` and `decode` methods. If no extension is present on the filename, `.json` will be added.

## spurt\_json

- spurt\_json $data, $filename, \[$json\_encoder\]

    # values can be passed as refs
    spurt_json \@array, 'ref.json';

    # or as an array or hash (still passed as refs using prototypes)
    spurt_json @array, 'array.json';

    spurt_json %hash, 'hash.json';

    # You can pass your own JSON encoder
    spurt_json $ref, 'ref.json', JSON::PP->new->ascii->pretty;

    # If no extension is provided, ".json" will be used.
    # Writes to "ref.json";
    spurt_json $ref, 'ref';

This reads in JSON from a file and returns it as a Perl data structure (a reference, an array, or a hash). You can pass in your own JSON encoder/decoder as an optional argument, as long as it is blessed
and has `encode` and `decode` methods. If no extension is present on the filename, `.json` will be added.

# OBJECT-ORIENTED INTERFACE

## new

    my $json_slurper = JSON::Slurper->new;

    # or pass in your own JSON encoder/decoder
    my $json_slurper = JSON::Slurper->new(JSON::PP->new->ascii->pretty);

["new"](#new) creates a [JSON::Slurper](https://metacpan.org/pod/JSON::Slurper) object that allows you to use the ["OBJECT-ORIENTED INTERFACE"](#object-oriented-interface) and call ["slurp"](#slurp) and ["spurt"](#spurt). You may pass in your own JSON encoder/decoder as long as it has
`encode` and `decode` methods, like [JSON::PP](https://metacpan.org/pod/JSON::PP) or [Cpanel::JSON::XS](https://metacpan.org/pod/Cpanel::JSON::XS), and this encoder will be used instead of the default one when calling ["slurp"](#slurp) and ["spurt"](#spurt).

## slurp

- slurp($filename)

    # values can be returned as refs
    my $ref = $json_slurper->slurp('ref.json');

    # or as an array or hash
    my @array = $json_slurper->slurp('array.json');

    my %hash = $json_slurper->slurp('hash.json');

    # If no extension is provided, ".json" will be used.
    # Reads from "ref.json";
    my $ref = $json_slurper->slurp('ref');

This reads in JSON from a file and returns it as a Perl data structure (a reference, an array, or a hash).
If no extension is present on the filename, `.json` will be added.

## spurt

- spurt($data, $filename)

    $json_slurper->spurt(\@array, 'array.json');

    $json_slurper->spurt(\%hash, 'hash.json');

    # If no extension is provided, ".json" will be used.
    # Writes to "ref.json";
    $json_slurper->spurt($ref, 'ref');

This reads in JSON from a file and returns it as a Perl data structure (a reference, an array, or a hash).
If no extension is present on the filename, `.json` will be added.

# TODO

More testing required.

# AUTHOR

Adam Hopkins <srchulo@cpan.org>

# COPYRIGHT

Copyright 2019- Adam Hopkins

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO

- [File::Slurper](https://metacpan.org/pod/File::Slurper)
- [JSON::PP](https://metacpan.org/pod/JSON::PP)
- [Cpanel::JSON::XS](https://metacpan.org/pod/Cpanel::JSON::XS)
