**Delta encoding** is a way of storing or transmitting [data](https://en.wikipedia.org/wiki/Data) in the form of *differences* (deltas) between sequential data rather than complete files; more generally this is known as [data differencing](https://en.wikipedia.org/wiki/Data_differencing). Delta encoding is sometimes called **delta compression**, particularly where archival histories of changes are required (e.g., in [revision control software](https://en.wikipedia.org/wiki/Revision_control_software)).

The differences are recorded in discrete files called "deltas" or "diffs". In situations where differences are small – for example, the change of a few words in a large document or the change of a few records in a large table – delta encoding greatly reduces data redundancy. Collections of unique deltas are substantially more space-efficient than their non-encoded equivalents.

From a logical point of view the difference between two data values is the information required to obtain one value from the other – see [relative entropy](https://en.wikipedia.org/wiki/Relative_entropy). The difference between identical values (under some [equivalence](https://en.wikipedia.org/wiki/Equivalence_relation)) is often called *0* or the [neutral element](https://en.wikipedia.org/wiki/Neutral_element).



## Simple example

Perhaps the simplest example is storing values of bytes as differences (deltas) between sequential values, rather than the values themselves. So, instead of 2, 4, 6, 9, 7, we would store 2, 2, 2, 3, −2. This reduces the [variance](https://en.wikipedia.org/wiki/Variance) (range) of the values when neighbor samples are correlated, enabling a lower bit usage for the same data. [IFF](https://en.wikipedia.org/wiki/Interchange_File_Format) [8SVX](https://en.wikipedia.org/wiki/8SVX) sound format applies this encoding to raw sound data before applying compression to it. Unfortunately, not even all 8-bit sound [samples](https://en.wikipedia.org/wiki/Sampling_%28signal_processing%29) compress better when delta encoded, and the usability of delta encoding is even smaller for 16-bit and better samples. Therefore, compression algorithms often choose to delta encode only when the compression is better than without. However, in video compression, delta frames can considerably reduce frame size and are used in virtually every video compression [codec](https://en.wikipedia.org/wiki/Codec).

## Definition

A delta can be defined in 2 ways, *symmetric delta* and *directed delta*. A *symmetric delta* can be expressed as:

> ​                    Δ        (                  v                      1                          ,                  v                      2                          )        =        (                  v                      1                          ∖                  v                      2                          )        ∪        (                  v                      2                          ∖                  v                      1                          )              {\displaystyle \Delta (v_{1},v_{2})=(v_{1}\setminus v_{2})\cup (v_{2}\setminus v_{1})}  ![{\displaystyle \Delta (v_{1},v_{2})=(v_{1}\setminus v_{2})\cup (v_{2}\setminus v_{1})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/bae3686d3f2f06a8174a8861acb17ea10b1f0a32)

where                               v                      1                                {\displaystyle v_{1}}  ![v_{1}](https://wikimedia.org/api/rest_v1/media/math/render/svg/98d33f5d498d528bd8c10edc8ac8c34347f32b3a) and                               v                      2                                {\displaystyle v_{2}}  ![v_{2}](https://wikimedia.org/api/rest_v1/media/math/render/svg/eb04c423c2cb809c30cac725befa14ffbf4c85f3) represent two versions.

A *directed delta*, also called a change, is a sequence of (elementary) change operations which, when applied to one version                               v                      1                                {\displaystyle v_{1}}  ![v_{1}](https://wikimedia.org/api/rest_v1/media/math/render/svg/98d33f5d498d528bd8c10edc8ac8c34347f32b3a), yields another version                               v                      2                                {\displaystyle v_{2}}  ![v_{2}](https://wikimedia.org/api/rest_v1/media/math/render/svg/eb04c423c2cb809c30cac725befa14ffbf4c85f3) (note the correspondence to [transaction logs](https://en.wikipedia.org/wiki/Transaction_log) in databases).

### Variants

A variation of delta encoding which encodes differences between the [prefixes](https://en.wikipedia.org/wiki/Prefix_%28computer_science%29) or [suffixes](https://en.wikipedia.org/wiki/Suffix_%28computer_science%29) of [strings](https://en.wikipedia.org/wiki/String_%28computer_science%29) is called [incremental encoding](https://en.wikipedia.org/wiki/Incremental_encoding). It is particularly effective for sorted lists with small differences between strings, such as a list of [words](https://en.wikipedia.org/wiki/Word) from a [dictionary](https://en.wikipedia.org/wiki/Dictionary).

## Implementation issues

The nature of the data to be encoded influences the effectiveness of a particular compression algorithm.

Delta encoding performs best when data has small or constant variation; for an unsorted data set, there may be little to no compression possible with this method.

In delta encoded transmission over a network where only a single copy of the file is available at each end of the communication channel, special [error control codes](https://en.wikipedia.org/wiki/Error-correction) are used to detect which parts of the file have changed since its previous version. For example, [rsync](https://en.wikipedia.org/wiki/Rsync) uses a rolling checksum algorithm based on Mark Adler's [adler-32](https://en.wikipedia.org/wiki/Adler-32) checksum.

## Sample C code

The following [C](https://en.wikipedia.org/wiki/C_%28programming_language%29) code performs a simple form of delta encoding and decoding:

```
void delta_encode(char *buffer, int length)
{
    char last = 0;
    for (int i = 0; i < length; ++i)
    {
        char current = buffer[i];
        buffer[i] = current - last;
        last = current;
    }
}

void delta_decode(char *buffer, int length)
{
    char last = 0;
    for (int i = 0; i < length; ++i)
    {
        char delta = buffer[i];
        buffer[i] = delta + last;
        last = buffer[i];
    }
}

```

## Examples

### Delta encoding in HTTP

Another instance of use of delta encoding is [RFC 3229](https://tools.ietf.org/html/rfc3229), "Delta encoding in HTTP", which proposes that [HTTP](https://en.wikipedia.org/wiki/HTTP) servers should be able to send updated Web pages in the form of differences between versions (deltas), which should decrease Internet traffic, as most pages change slowly over time, rather than being completely rewritten repeatedly:

> This document describes how delta encoding can be supported as a compatible extension to HTTP/1.1.
>
> Many HTTP (Hypertext Transport Protocol) requests cause the retrieval of slightly modified instances of resources for which the client already has a cache entry. Research has shown that such modifying updates are frequent, and that the modifications are typically much smaller than the actual entity. In such cases, HTTP would make more efficient use of network bandwidth if it could transfer a minimal description of the changes, rather than the entire new instance of the resource.

### Delta copying

*Delta copying* is a fast way of copying a file that is partially changed, when a previous version is present on the destination location. With delta copying, only the changed part of a file is copied. It is usually used in [backup](https://en.wikipedia.org/wiki/Backup) or [file copying](https://en.wikipedia.org/wiki/File_copying) software, often to save [bandwidth](https://en.wikipedia.org/wiki/Bandwidth_%28computing%29) when copying between computers over a private network or the internet.[[1\]](https://en.wikipedia.org/wiki/Delta_encoding#cite_note-1)[[2\]](https://en.wikipedia.org/wiki/Delta_encoding#cite_note-2)[[3\]](https://en.wikipedia.org/wiki/Delta_encoding#cite_note-3)

### Online backup

Main article: [Online backup services](https://en.wikipedia.org/wiki/Online_backup_services)

Many of the [online backup services](https://en.wikipedia.org/wiki/Online_backup_services) adopt this methodology, often known simply as *deltas*, in order to give their users previous versions of the same file from previous backups. This reduces associated costs, not only in the amount of data that has to be stored as differing versions (as the whole of each changed version of a file has to be offered for users to access), but also those costs in the uploading (and sometimes the downloading) of each file that has been updated (by just the smaller delta having to be used, rather than the whole file).

### Git

Main article: [Git (software)](https://en.wikipedia.org/wiki/Git_%28software%29)

The Git source code control system employs delta compression in an auxiliary "[git repack](http://git-scm.com/docs/git-repack)" operation. Objects in the repository that have not yet been delta-compressed ("loose objects") are compared against a heuristically chosen subset of all other objects, and the common data and differences are concatenated into a "pack file" which is then compressed using conventional methods. In common use cases, where source or data files are changed incrementally between commits, this can result in significant space savings. The repack operation is typically performed as part of the "[git gc](http://git-scm.com/docs/git-gc)" process, which is triggered automatically when the numbers of loose objects or pack files exceed configured thresholds.

### VCDIFF

Main article: [VCDIFF](https://en.wikipedia.org/wiki/VCDIFF)

One general format for delta encoding is VCDIFF, described in [RFC 3284](https://tools.ietf.org/html/rfc3284). [Free software](https://en.wikipedia.org/wiki/Free_software) implementations include [Xdelta](https://en.wikipedia.org/wiki/Xdelta) and open-vcdiff.

### GDIFF

Generic Diff Format (GDIFF) is another delta encoding format. It was submitted to [W3C](https://en.wikipedia.org/wiki/W3C) in 1997.[[4\]](https://en.wikipedia.org/wiki/Delta_encoding#cite_note-4) In many cases, VCDIFF has better compression rate than GDIFF.

### Diff

Main article: [Diff](https://en.wikipedia.org/wiki/Diff)

Diff is a file comparison program, which is mainly used for text files.

### bsdiff

[Bsdiff](http://www.daemonology.net/bsdiff/) is a binary diff program using [suffix sorting](https://en.wikipedia.org/wiki/Suffix_array).

## See also

- [Data differencing](https://en.wikipedia.org/wiki/Data_differencing)
- [Interleaved deltas](https://en.wikipedia.org/wiki/Interleaved_deltas)
- [Source Code Control System](https://en.wikipedia.org/wiki/Source_Code_Control_System)
- [String-to-string correction problem](https://en.wikipedia.org/wiki/String-to-string_correction_problem)
- [Xdelta](https://en.wikipedia.org/wiki/Xdelta): open-source delta encoder

## References


 [http://www.2brightsparks.com/bb/viewtopic.php?t=4473](http://www.2brightsparks.com/bb/viewtopic.php?t=4473)

 [https://www.bvckup2.com/support/forum/topic/739](https://www.bvckup2.com/support/forum/topic/739)

 [http://www.eggheadcafe.com/software/aspnet/33678264/delta-copying.aspx](http://www.eggheadcafe.com/software/aspnet/33678264/delta-copying.aspx)

1.  [Generic Diff Format Specification](http://www.w3.org/TR/NOTE-gdiff-19970901)

## External links

- [RFC 3229](https://tools.ietf.org/html/rfc3229) – Delta Encoding in HTTP