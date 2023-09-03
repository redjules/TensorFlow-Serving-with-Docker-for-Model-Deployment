# Dataset

Available at https://www.kaggle.com/snap/amazon-fine-food-reviews/data.
We only use a training subset.

# What it does

This code creates a model that, given a product review, says if it is positive, neutral or negative. It is trained with real reviews and their associated star ratings (1-5 stars).
The model is then deployed with Docker so that we can interact with it through REST or gRPC requests.

# How to use

The Jupyter notebook contains some cells with lines such as

    %%writefile -a train.py

By running these cells with this line uncommented, the relevant code is copied to python files that we can run individually, rather than working from the notebook.

Open a console and run this script which will create, train, and save the model in /amazon \_review folder:

    python3 train.py

Open a console and run these commands to download the Docker image and to run it:

    docker pull tensorflow/serving

    docker run -p 8500:8500 \
            -p 8501:8501 \
            --mount type=bind,\
            source=/path/amazon_review/,\
            target=/models/amazon_review \
            -e MODEL_NAME=amazon_review \
            -t tensorflow/serving

Leave the console window running with the docker image executing.

Now we have a server actively listening to REST or PRC requests. We can send a review and it will respond with 3 confidence values on wether it is positive, neutral or negative.

Now run, on another console window

    tf_serving_rest_client.py

or

    tf_serving_grpc_client.py

to start sending requests with reviews.
