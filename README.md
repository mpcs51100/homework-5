# Homework 5

_Due: Sunday, Nov. 22 @6pm_

Imagine you have a large document with many repeated long strings. A simple way to compress the file is to replace frequently occurring words with a simple abbreviation. For example in the text:

>But good morning! Good morning to ye and thou! I’d say to all my patients, because I was the worst of the hypocrites, of all the hypocrites, the cruel and phony hypocrites, I was the very worst.

If we create a simple rule, e.g., that all words with at least four characters and one or more occurrence are encoded, then we might encode according the following abbreviation table:

| word        | occurrences | byte code |
|-------------|-------------|-----------|
| hyprocrites |      3      |     1     |
| morning     |      2      |     2     |
| good        |      2      |     3     |
| worst       |      2      |     4     |

and thus the encoded document becomes:

>But 3 2! 3 2 to ye and thou! I’d say to all my patients, because I was the 4 of the 1, of all the 1, the cruel and phony 1, I was the very 4.

Of course, the compressed file would also have to encode the translation table so that it could be uncompressed.

Consider three implementations of this simple algorithm, each of which requires O(N) space, where N is the number of strings in the document:
 
 * naive solution with O(N^2) time complexity.
 * a binary tree solution. Self-balancing isn’t required in this exercise, but it should be recognized that an R-B balancing is simple to add if needed since no deletions are involved.
 * a hashmap solution that can work with any hash function. For the hash solution test at least each of the following:

```
unsigned int naive_hash(char *word, int nbins)
{
    unsigned int h = 0;
    int c;
    while(c = *word++)
        h += c;
    return h % nbins;
}
```
```
unsigned int bernstein_hash(char *word, int nbins)
{
    unsigned int h = 5381;
    int c;
    while(c = *word++)
        h = 33 * h + c;
    return h % nbins;
}
```
```
unsigned int FNV_hash(char *word, int nbins)
{
    unsigned long h = 14695981039346656037lu;
    char c;
    while(c = *word++)
    {
        h = h * 1099511628211lu;
        h = h ^ c;
    }
    return h % nbins;
}
```
You can implement your hashmap with open addressing, chaining, or multi-level hashing. Allocate N buckets where N is the number of words in the text. The code should output either an encoded document given the original input document, or the reverse (uncompress an encoded document). Your code should also take as input the threshold string length and frequency required to trigger encoding. Verify the accuracy of each method, and do a performance comparison for the long and short input files provided. Design clear interfaces and include meaningful comments. Be prepared to discuss your performance results and design choices in class. 
