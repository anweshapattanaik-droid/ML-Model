# ML-Model

what i built-
I built a small image classifier that can tell the difference between a watch and a pen. I took 30 photos of each object using my phone, so 60 images total. The photos were taken from different angles. I trained a small CNN (Convolutional Neural Network) on this dataset and ran 4 different experiments to see how changing certain settings affects the training.



Dataset-
i built my dataset by taking photos in my phone of pen and watch in different background .However, most images had similar backgrounds, which influenced the model behavior. 80% images are used for training and 20% for validation.
As my dataset is very large so i am uploading the google drive link here - https://drive.google.com/drive/folders/1Xm1F0qJ_iK1eif1p34uc_wfwf7zRimgi?usp=sharing

experiments-
1- experiment settings - Learning Rate = 0.001, Batch Size = 16, Dropout = 0.0, The model started slow at epoch 5 it was still at 50% which is basically random guessing. But by epoch 10 it figured it out and hit 100% validation accuracy. Loss was going down smoothly throughout. This became my reference point for everything else.
2-experiment settings: Learning Rate = 0.1, Batch Size = 16, Dropout = 0.0. This experiment completely failed. The model was stuck at 50% accuracy for all 20 epochs meaning it never learned anything.I think what happened is that the learning rate of 0.1 was way too large. During backpropagation, the optimizer updates the weights by a step proportional to the learning rate. When that step is too big, the model keeps overshooting the minimum of the loss function and never settles. You can also see in the loss graph that the High LR line started extremely high (around 560) in the first epoch before crashing that initial spike shows how unstable the gradients were.
3-experiment settings-LR = 0.001, Batch Size = 16, Dropout = 0.5. I experiment this  by adding dropouts .I added dropout of 0.5. Interestingly the model still reached 100% accuracy but what stood out is the val loss at epoch 20 was only 0.012 which is very low. The train and val loss were also very close to each other throughout, meaning the model was not overfitting ,it was generalizing well.
4-experiment settings: LR = 0.001, Batch Size = 4, Dropout = 0.0.With batch size 4, the model updates its weights after every 4 images instead of 16. This means more updates per epoch which might be why it converged faster by epoch 10 the loss was already at 0.011 which is much lower than the baseline at the same epoch. However looking at the accuracy graph it was very jumpy in the early epochs (going from 0.67 to 1.0 to 0.67 in the first few epochs), which shows that small batches make training noisier. It still ended up at 100% accuracy with a very low loss.

What I Observed-
Learning rate is probably the most important hyperparameter. Setting it too high completely broke the training(ref exp 2)the model never learned anything across all 20 epochs.
Dropout helped the model generalize better. The val loss with dropout (0.012) was much lower than the baseline val loss (0.279) at the end, even though both hit 100% accuracy.(ref exp 3)
Small batch size made training noisier early on but the model ended up converging to a lower final loss than the baseline.(exp 4)
All experiments except the high LR one eventually reached 100% validation accuracy. This is mostly because the validation set is only about 12 images, so it is easy to classify all of them correctly once the model has learned basic features.


Things That Confused Me
1-At first I did not understand why the high learning rate caused the loss to start at 560 in the first epoch. After thinking about it I realized the weights were being updated so aggressively that the model outputs became very large numbers, making the loss explode before it eventually came down (but never actually learned).
2-I was confused why dropout slowed down early training (still 50% at epoch 5) but ended up with the best final val loss. I think it is because dropout makes each training step harder, so the model takes longer to start learning, but what it learns is more robust.
