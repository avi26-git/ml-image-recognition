# Download the Open Images TensorFlow model and extract it to the `model`
# folder.
FROM alpine AS builder
RUN mkdir model
RUN wget -c https://storage.googleapis.com/tfhub-modules/google/openimages_v4/ssd/mobilenet_v2/1.tar.gz -O - | tar xz -C model
# Make sure it's world-readable so the Lambda service user can access it.
RUN chmod -R a+r model
​
# Build the runtime image from the official AWS Lambda Python base image.
FROM public.ecr.aws/lambda/python
# Copy the extracted Open Images model into the source code space.
COPY --from=builder model model
# Copy in the sources.
COPY handler.py requirements.txt ./
# Copy the OpenSans font for annotation use
COPY font ./font
# Install the Python dependencies
RUN python3 -m pip install -r requirements.txt
# Tell the Lambda runtime where the function handler is located.
CMD ["handler.lambda_handler"]
