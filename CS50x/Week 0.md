# [Unicode](https://cs50.harvard.edu/x/2024/notes/0/#unicode)

- Since there were not enough digits in binary to represent all the various characters that could be represented by humans, the _Unicode_ standard expanded the number of bits that can be transmitted and understood by computers. Unicode includes not only special characters, but emoji as well.
- Computer scientists faced a challenge when wanting to assign various skin tones to each emoji to allow the communication to be further personalized. In this case, the creators and contributors of emoji decided that the initial bits would be the structure of the emoji itself, followed by skin tone.
- For example, the unicode for a generic thumbs up is `U+1F44D`. However, the following represents the same thumbs up with a different skin tone: `U+1F44D U+1F3FD`.

# [Representation](https://cs50.harvard.edu/x/2024/notes/0/#representation)

- Zeros and ones can be used to represent color.
- Red, green, and blue (called `RGB`) is a combination of three numbers.
    
    ![red green blue boxes](https://cs50.harvard.edu/x/2024/notes/0/cs50Week0Slide118.png "red green blue boxes")


# [Pseudocode](https://cs50.harvard.edu/x/2024/notes/0/#pseudocode)

- Pseudocode is a human-readable version of your code.
- Pseudocoding is such an important skill for at least two reasons. First, when you pseudocode before you create formal code, it allows you to think through the logic of your problem in advance. Second, when you pseudocode, you can later provide this information to others that are seeking to understand your coding decisions and how your code works
