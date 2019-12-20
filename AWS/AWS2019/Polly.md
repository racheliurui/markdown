title: AWS - Polly
date: 2019-07-23 11:37:23
tags:
- AWS
- Polly
---

https://aws.amazon.com/blogs/machine-learning/build-your-own-text-to-speech-applications-with-amazon-polly/#


* Lambda changed to 3.7
  * 2 lines of code need to be updated.

```Python
print ("Text to Speech function. Post ID in DynamoDB: " + postId)
```

```python
#In Python 3 it makes a difference whether you open the file in binary or text mode. Just add the b flag to make it binary:
with open(output, "ab") as file:
    file.write(stream.read())
```
