## RASP_Pset

This repo includes several problems and their solutions about the RASP program (https://github.com/tech-srl/RASP).

### Problem 1:
Write an s-op that contains the indices in reverse order.
For example:
```
> reverse_indices("hello")
[4, 3, 2, 1, 0]
```
**Answer 1**:
```
reverse_indices = length - 1 - indices;
```

### Problem 2:
Write a function that "rotates" the input text by the specified number of characters.
For example:
```
> rotate(tokens, 0)("hello")
"hello"
> rotate(tokens, 1)("hello")
"ohell"
> rotate(tokens, 2)("hello")
"lohel"
```
**Answer 2**:
```
def rotate(seq, pos){
	rotate_indices = indices+pos if indices+pos < length else indices+pos-length;
	return aggregate(select(rotate_indices, indices, ==), tokens);
}
```

### Problem 3:
Write a function that takes a sequence as input and "swaps every letter with its neighbor".
Specifically, for every even index $i$, positions $i$ and $i+1$ will be swapped;
if the length of the sequence is odd, then the last element should not move.
For example:
```
> swap(tokens)("hello")
"ehllo"
> swap(tokens)("ababab")
"bababa"
```
**Answer 3**:
```
def swap(seq){
	switch = indices+1 if indices%2 == 0 else indices-1;
	check = switch if indices < round(length/2)*2 else indices;
	return aggregate(select(check, indices, ==), tokens);
}
```

### Problem 4:
Write a function that returns the maximum value in the sequence repeated for every position.
For example:
```
> maxseq(tokens)("ababcabab")
"ccccccccc"
```
**Answer 4**:
```
def maxseq(seq){
	sorted = sort(tokens, tokens);
	return aggregate(select(indices, length-1, ==), sorted);
}
```

### Problem 5:
Write a function that performs sequence reversal "autogeneratively".
That is, it will take a sequence as input, and the sequence will contain a special token `$` that marks the "end of the prompt".
The text before the `$` should be unchanged, and the text after the `$` should be the text before the `$` reversed (this text represents the model's response to the prompt).
The code should be robuse to the case when the length of text after `$` is not the same as the length of text before `$`.
For example:
```
> reverse_ag(tokens)("hello$     ")
"hello$olleh"
> reverse_ag(tokens)("hello$ ")
"hello$o"
> reverse_ag(tokens)("hello$X")
"hello$o"
> reverse_ag(tokens)("hello$XXXXXXXXXX")
"hello$olleh     "
```
**Answer 5**:
```
def reverse_ag(seq){
	pos = aggregate(select(tokens, "$", ==), indices);
	reverse = aggregate(select(indices, indices if indices <= pos else pos - indices % (pos * 2+1) + pos, ==), tokens);
	return reverse if indices <= pos*2 else " ";
}
```

### Problem 6:
Write a function that counts the number of times a certain token appears in the input sequence.
For example:
```
> howmany(tokens, "a")("hello")
"00000"
> howmany(tokens, "h")("hello")
"11111"
> howmany(tokens, "l")("hello")
"22222"
```
**Answer 6**:
```
def howmany(seq, key){
	return selector_width(select(seq, key, ==));
}
```

### Problem 7:
Write a function that counts the number of times a certain token has appeared in the input sequence so far.
For example:
```
> howmany(tokens, "a")("hello")
"00000"
> howmany(tokens, "h")("hello")
"11111"
> howmany(tokens, "e")("hello")
"01111"
> howmany(tokens, "l")("hello")
"00122"
```
**Answer 7**:
```
def howmany(seq, key){
	return selector_width(select(seq, key, ==) and select(indices, indices, <=));
}
```

### Problem 8:
Write a function that determines whether all the tokens are lowercase alphabet.

**Answer 8**:
```
def only_low_alpha(seq) {
	low_alpha_num = selector_width(select(tokens, "a", >=) and select(tokens, "z", <=));
	return length == low_alpha_num;
}
```