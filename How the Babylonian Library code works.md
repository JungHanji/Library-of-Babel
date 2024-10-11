# Basic concepts

1. **There should be a search for an address**
2. **There should be a content search**
3. **Random address generation**
4. **Title generation**
5. **Search all possible books in the library**
6. **Calculating the number of search results**
# Address search

**Entry is given:**
- HEX-address (i.e., the address of the room)
- Wall number
- Shelf number
- Book number
- Page number

1. The *address of a particular object in a library section* is calculated by adding up the *string* values of page, book, shelf and wall. If the *page* has less than 3 digits, a zero is added to the beginning to match the 3-digit size. The same happens with the book number, but in this case the size is 2 digits

$$
A = int(P + B + S + W)
$$

2. The *seed* for the book is then calculated. It must be unique, and so its formula is quite complicated. It is calculated as:

$$
S = int(hexBase) - libCoodinate \cdot charsetLen^{maxSimbols \cdot maxLines} 
$$

- *hexBase* — HEX part of this address
- *libCoordinate* — Library section coordinate from item *1*
- *charsetLen* — Number of characters in the text generation set
- *maxSimbols* — Maximum number of characters per line
- *maxLines* — Maximum number of lines per page

3. After the seed, the address itself is calculated. To do this, first the “link to the seed” is calculated by converting the seed into a 36-digit number system, where the characters are the sum of the English alphabet and 10 digits. And then the address is calculated as

$$A = base(S, charsetLen)$$
4. Then cases where the number of characters is less than the maximum number of characters per page are handled. This is done by setting the seed for the pseudo-random number generator, and then starting a loop that fills the free spaces in the text by adding a random letter from the letter set

5. Cases where the number of characters is greater than the maximum number of characters per page are also handled. In this case, letters exceeding the limit are cut off from the result

# Content Search

> *An important caveat: this function does not search the entire library for text, only a specific location.*


**Entry is given:**
- Text for search
- Section coordinate in the library

1. If the text length is less than the maximum, we fill it randomly based on the section coordinate (in the same way as with the address search). 
2. Calculate the HEX address, i.e. the numerical representation of the text:
	1. The loop goes through the entire text in reverse, calculating the literal value and adding it to the numeric value, after some mathematical operations
	2. The literal value is calculated by a simple algorithm:
		1. If a letter from the text is contained in ASCII, the letter value is calculated as: the numeric equivalent of the letter minus 97 (the character code “a” in the ASCII table) $$C = (int)ch - 97$$
		2. Otherwise, the literal value is equal to the index of that character in the literal set $$C = i$$
	3. The letter value is added to the numeric value multiplied by the length of the letter set to the extent of *the letter index from the original text (i.e., loop iteration)*. $$S = S + C \cdot charsetLen^{i}$$
3. The address is calculated as the coordinate of the library section multiplied by the letter set length times the maximum number of characters per page plus the numerical representation of the text $$A = libCoord \cdot charsetLen^{maxCharsetLen} + S$$
4. After the address is converted to 36-digit number system 
$$A = base(A, 36)$$

# Book titles

???

# Search through all books in the library

???
# Generating a random address

???

# Calculating the number of search results

???

# Main operating principle
The code doesn't actually *exactly* search for a given text in the entire Babel library, but generates either an address based on the context that you gave to that algorithm, or generates text that matches the address.