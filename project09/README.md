# Introduction
This project focuses on implementing Forward, Backward, and Forward-Backward algorithms, which are fundamental to the Hidden Markov Model (HMM). While Project 08 focused on finding the single most likely path via Viterbi, Project 09 shifts to calculating sequence likelihoods and posterior marginal probabilities. This implementation provides hands-on experience in determining the probability of being in each hidden state at each position given the entire observation sequence, which is critical for biological sequence analysis and probabilistic inference.


# Pseudocode
Put pseudocode in this box:

```
Forward-Backward Algorithm

Class Forwardbackward(HMM):
    
    Method forward(sequence of observations):

        # Initialization
        Create a forward_matrix of size [states] x [number of observations]

        For each state 's':
            forward_matrix[s][0] = log_initial[s] + log_emission[s][0]

        # Iteration
        For each time step 't' from 1 to sequence_length - 1:
            For each current_state: 
                initialize an empty list values to store the probabilities that need to be summed up for each observation
            
                #evaluate all possible paths from the previous state
	    		For each previous state: 
		    	    append the sum of the probability in the forward matrix for t-1 at the previous state, log_transition of previous state to current state and log_emission of observation at t at current state to values
			
                # sum up the list of values to get the probability at t at current state (use np.logaddexp.reduce)
                forward_matrix[current_state][t] = np.logaddexp.reduce(values)
            
        total_forward_prob = logaddexp over forward_matrix[s][n-1] for all s    # n-1 is the last observation

        Return the forward_matrix and total probability


        Method backward(sequence of observations):

        # Initialization
        Create a backward_matrix of size [states] x [number of observations]

        For each state 's':
            backward_matrix[s][n-1] = 0.0     # Since we need to initialize the last observation to 1, but we are working in log-space so log(1) = 0.0

        # Iteration
        For each time step 't' from the n-2 to 0:
            For each current_state: 
                initialize an empty list values to store the probabilities that need to be summed up for each observation
            
	    		For each next_state: 
		    	    append the sum of the probability in the backward matrix for t+1 at the next_state, log_transition of current_state to next_state and log_emission of observation at t+1 at nextt_state to values
                
                # sum up the list of values to get the probability at t at current state (use np.logaddexp.reduce)
                backward_matrix[current_state][t] = np.logaddexp.reduce(values)

        final_values = [(backward_matrix[s][0] + log_emission[s][0] + log_initial[s]) for s in self.states]     # Include initial probabilities and emission probability of first observation
        total_backward_probability = np.logaddexp(final_values)

        Return the backward_matrix and total probability


        Method run(sequence of observations):
            Get the forward_matrix and total_forward_probability from the forward method
            Get the backward_matrix and total_backward_probability from the backward method

            Initialize a final_probability_matrix of size [states] x [number of observations]
            # Compute combined forward-backward probabilities (posterior in normal space)
            For t = 0 to n-1:
                For each state s:
                    final_prob_matrix[s][t] = exp(forward_matrix[s][t] + backward_matrix[s][t] - total_forward_prob)

        Return forward_matrix, total_forward_prob, backward_matrix, total_backward_prob, final_prob_matrix
        
```

# Successes
* We were successfully able to use our Object-Oriented Programming (OOP) architecture from Project 08, making the integration of the new inference algorithms seamless
* We implemented np.logaddexp, which provided a robust way to prevent numerical underflow when dealing with these sequences
* A major success was verifying our implementation by comparing the total sequence probability from both the Forward and Backward passes, which yielded nearly identical results as required.

# Struggles
* One of the main challenges we faced was working consistently in log-space. It was sometimes confusing to keep track of when values needed to be in log space and when they needed to be in normal space. This was especially confusing when we initially discussed summing up the probabilities of an observation across all states, but the use of `np.logaddexp` helped simplify the process. 

* Another minor difficulty we had was coding the backward algorithm, although conceptually it was easy to understand, we did get a little confused about which probabilities needed to be added while we were coding.

# Personal Reflections
## Group Leader
Fardina Tabassum - Overall this project was a bit more straightforward to start as we had classes made from the previous project to work with. I was initially confused if we were supposed to do posterior decoding or forward-backward algorithm for this project but then we quickly got that sorted out. The hardest part was ensuring that our data structures remained flexible enough to handle any number of states or symbols, as the instructions warned us to make no assumptions about the model's scale. It was also nice to see that we could verify our algorithms by seeing if our Forward and Backward sequence likelihoods matched. 

## Other member
Connor Crawford - Given the similarity of this project to the last project we did with the Viterbi algorithm, developing the pseudocode and grasping the conceptual portion of the forward backward algorithm felt relatively smooth. Especially where our group had established an HMM class structure previously to build off. One of the most challenging parts for me was determining how to sum increasingly small probabilites in a stable manner. We had to be careful when dealing with the sums in probability space because you'd quickly run into a underflow error, conversely, you couldn't just add log values because that would be mathematically incorrect. Additionally working backwards over the matrix to get marginal probabilities in the backward algorithm was a little bit awkward, but since we were doing the same thing we already implemented in the forward algorithm it wasn't too hard to figure out.

Meghana Ravi - This project was conceptually similar to the Viterbi implementation so I was able to focus more on the coding and design aspect, especially how object-oriented programming is beneficial. This was the first time I've ever implemented an algorithm where I reused parts of another code instead of just rewriting functions. This made me realize how designing code to be more general and reusable saves time and also makes the entire process of implementing complex algorithms more efficient. Logically, I did need a little bit of time to break down the details of the backward algorithm and the use of `np.logaddexp`, but overall it was fairly simple to understand.

# Generative AI Appendix
As per the syllabus
