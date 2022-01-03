* Reading Text: 
In this question we need to recognize text in the image, character by character, but images given images are noisy which makes it difficult for the text in the image to be recognized with accuracy.
* Assumptions:
  *	Image has English words and sentences.
  *	 Image has the same fixed-width font of the same size. i.e. each letter is in a box that is 16 pixels wide and 25 pixels tall.
  *	Another assumption we take here is that we consider only 26 uppercase Latin characters, 26 lowercase characters, 10 digits, spaces, 7 punctuations symbols.
* Initial code: The skeletal code provided does the I/O of the image, it converts the image into list of lists that represents a 2 grid of black and white dots.
* Approach:

We use bc.train file from the part 1, but by skipping the even positions which will help us ignore the words  pos tag , to calculate initial, emission and transitional probabilities.
 
For calculating the emission probability, we keep a count of various factors:
  * black_count_test: has the count of the ‘*’ that occurs in the in the test data
  *	black_count_train: has the count of the ‘*’ that occurs in the in the train data
  *	black: has the count of ‘*’ that are matching of the word that we get in the image and what we get in the train data file
  *	white: has the count of ‘ ’ that are matching of the word that we get in the image and what we get in the train data file
  *	no_match_black: has the count of ‘*’ that do not match from the image/test and in train file
  *	no_match_white: has the count of ‘ ’ that do not match from the image and in train file

For calculating the emission probability, we tried different weights for all variables , keeping in mind he density of the pixels, and after much experimentation we settled on the following weights, which gave us a more accurate reading of the text in the image. 

if the probability of getting ‘*’ count  in the test data is more than that of the probability of ‘*’ in train data then we have weights as :
  *	75% for ‘*’ i.e black variable
  *	75%, of white i.e ‘ ’ 
  *	25% for both  no_match_black and no_match_white.

Else the weights will be as follows:
  * 97% for ‘*’ i.e black variable
  *	75% for ‘ ’ i.e white variable
  *	25% for ‘*’ i.e no_match_black variable
  *	3% for no_match_white variable

#### Simplified Bayes Net: 
In this approach, using emission probability, we get the character that has the maximum probability, which we join into the resultant string and give that as output.

#### Hidden Markov Models-Viterbi:

Here we are maintaining 2 lists, with names current_letter and prev_letter which helps us store the emission and transitional probabilities , initialized initially to None with a length of 128 as there are 128 characters in ASCII. This is done so that we can easily map the ASCII value to the character that we get in the image.
So here, we run though the letter in the train and test, for the first character in the test data, we calculate the probability, we take the negative log of the letter, we take negative log here  because to minimize the cost, and then we get the ASCII value (Unicode code value) of the train_letter using the ord function.
 
If the character is not first, we create a temp list, to calculate the transitional probability from the previous letter conditioned on the current letter and append the transitional probability to the temp list.

We then get the character that has the minimum cost value, we add it to the emission probability.
 Then from the list of cost,  we get the minimum cost which represents the maximum probability and print that out as the output.
