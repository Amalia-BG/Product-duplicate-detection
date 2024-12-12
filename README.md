# Product-duplicate-detection
Scalability in duplicate detection problem based on LSH (data set of TVs).
It is mostly based on the methodology of Hartveld, A., van Keulen, M., Mathol, D., van Noort, T., Plaatsman, T., Frasincar, F., Schouten, K.: An lsh-based model-words-driven product duplicate detection method. In: International Conference on Advanced Information Systems Engineering. pp. 409–423. Springer (2018).

But also includes some adaptations, which are marked with "Novel" description in the code.

This README is meant to guide you through the code.
Note, there are some commented(uncommented) parts which you need to uncomment(comment) in order to change the methodology type used.
There are also some functions marked with (Not used), which were not needed for the final run, but which were used in the creation process.

# Functions used
## get_brand_values(data_id)
Returns a list of the existing brand values, extracted from the data. The input data_id still contains modelID, by which the products can be identified.

## clean_and_normalize_original(text)
Normalizes a given 'text' input, based on the original methodology of the earlier mentioned paper.
## clean_and_normalize_novel(text)
Normalizes a given 'text' input, with some additional adaptations, the description of which can be found in the code.

## Extracting model words:
## extract_model_words_from_title_original(title); extract_model_words_from_kvp_original(features_map); extract_model_words_original_BOTH(data)
The first function extracts the model words from the title, while the second from the key-value-pairs (KVP) based on the paper. The last one combines the output of the first two functions.
## extract_model_words_from_title_novel(title); extract_model_words_from_kvp_novel(features_map); extract_model_words_from_brand(features_map); extract_model_words_novel_BOTH(data)
Same goal as the previously mentioned functions: to extract the model words; With some adaptations included. And the third function also extracts the model words associated with brand.

## Binary Matrix:
## get_binary_matrix(data, mw_per_product)
Uses as input the data in order to iterate over it, and the previously obtained combined model words per product. Uses these to return a binary matrix of products x model words, based on their occurence for a given product.
## get_binary_matrix_with_brand_influence(data, mw_per_product, brand_factor, data_id)
Same as before, forms and returns a binary matrix, but duplicates the columns of the original binary matrix associated with brand by a brand_factor.

## Signature Matrix:
## generate_unique_hash_params(k, prime)
Generates random values for parameters a and d used later in a hash function. k and prime are inputs high enough, based on previously found model words, in order to avoid collisions.
## hash_function(row, a, d, prime)
Defines a hash function
## minhash_signature_matrix(used_binary_matrix, k, prime)
Creates a signature matrix, based on the earlier obtained binary matrix, and hash function.

## LSH:
## jaccard_similarity(vec1, vec2)
Finds Jaccard similarity between 2 vectors
## lsh(b, r, given_signature_matrix)
Performs LSH, b bands, r rows on the signature matrix

## True Pairs:
## get_true_pairs_from_list(data_id)
Gets the number of true duplicate pairs in the data

## Clustering:
## hierarchical_clustering(data, used_binary_matrix, b, r, given_signature_matrix, used_method)
Performs hierarchical clustering, likage = used_method. Uses b and r for the threshold to assign products based on similarity above threshold. Outputs clusters, where products in the same cluster are considered duplicates.
## get_pairs_from_clusters(clusters)
Returns the 'found' duplicate pairs into pairs from the cluster representation

## Evaluation:
## bootstrap(data, data_id, n_bootstraps)
Splits the data into train and test, and keeps track of it. Makes n_boostrap sets of train, test data.
## evaluate_performance(used_pred_pairs_lsh, used_pred_pairs_clusters, used_true_pairs, used_current_possible_pairs)
Finds PQ, PC, F1, and F1* measures

# Main Run:
## (Initialise run)
Runs the part which is fixed for both train and test data.
Includes importing the data, transforming it from .json to list, and separating modelID used for evaluation, from the rest of the data.
Initialising the bootstraps, results matrices.
## Train data run
Runs for the train data and records the latest result
## Test data run
Runs for the test data and records the latest result
## Graphs
Graphs of the latest results

