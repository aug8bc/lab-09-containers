# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.

This is a bad idea because then anyone could read my credentials because they could pull any file that was copied into the image and this could lead to my intellectual property getting damaged or stolen. Also, when the learner lab ends, my credentials would become invalid and because they would be copied into the image I would have to rebuild the image everytime the credentials expired and changed.

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

The 'get_atlas_settings()' function looks for the MongoDB credentials in an if operater so when they weren't found with -e the function went to the next part and gets the credentials from the SSM parameter set in AWS, which has my mongo credentials from lab 7. This parameter was read using my AWS credentials, ultimately getting my mongo credentials to the container.
 
## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?

Before: 2d91ae012bb3
After: 896720e102e3

The IDs are different which is a problem because the tag, which people look at to identify an image, will not change so someone will be using an image that they think has different content which would be confusing and problematic. Images are supposed to be reproducible, but someone using your image may actually be running something different than you which defeats that purpose and it would be hard to identify what was causing different results if the tag was the same. Instead, the updated image should have a new tag to represent that it is a new version, like 0.1.1.
