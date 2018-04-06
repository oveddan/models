<p align="center">
<img src="utils/icon.png" width="100">
</p>

# Runway Models

This repo contains the current public models for [Runway](https//runwayml.com) that you can use from the app.

## About

`models.js` is the file that Runway queries when opened. It has all the necessary information to create and install models within the Runway.

Each model lives in its own folder and has its own configurations.

## Status

### Implemented

- [im2txt](https://github.com/tensorflow/models/tree/master/research/im2txt)
- [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose) (based on [tf-openpose](https://github.com/ildoonet/tf-pose-estimation))

### In Progress

- [YOLO](https://pjreddie.com/darknet/yolo/)

### Wish List

- DeepSpeech
- pix2pixHD
- Detectron

## How models work in Runway

### Docker Containers

Each model is a docker container thats creates a public HTTP socket server. Containers must public in the [Docker Hub](https://hub.docker.com/). More on this soon.

### Server 

Models running inside Docker can be work in any language or framework. They just need to be accesible through a public url. A server with socket connection must be created. 

### models.js 

The file `model.js` is a collection of meta information about docker container. This are the final models Runway queries and installs. A valid model must have the following format in `models.js`

```javascript
{
    containerId: String,
    tag: String,
    name: String,
    specs: {
      cpu: Boolean,
      gpu: Boolean,
    },
    port: String,
    query: {
      http: {
        route: String,
        method: String,
        type: String,
      },
      socket: {
        namespace: String,
        emit: String,
        event: String
      },
      format: {
        data: String
      }
    },
    size: [String, String],
    options: {
      image: String,
      Tty: Boolean,
      Detach: Boolean,
      ExposedPorts: { String: {} },
      HostConfig: {
        AutoRemove: true,
        PortBindings: {
          String: [{
            HostIP: String,
            HostPort: String
          }],
        },
      }
    },
    needsTraining: Boolean,
    description: {
      first: ,
      second: String,
      third: String,
      authors: String,
      image: String,
      organization: String,
      framework: String,
      github: String,
      paper: String,
      licence: String,
    },
    inputs: [String, String]
  }
```

For instance, this is the current implementation of im2txt:

```javascript
{
    containerId: '84d72a2f82f6',
    tag: 'cvalenzuelab/im2text:latest',
    name: 'im2txt',
    specs: {
      cpu: true,
      gpu: true,
    },
    port: '33100',
    query: {
      http: {
        route: 'query_once',
        method: 'post',
        type: 'JSON',
      },
      socket: {
        namespace: 'query',
        emit: 'update_request',
        event: 'update_response'
      },
      format: {
        data: 'data:image/jpg;base64,'
      }
    },
    size: ['2.19', 'GB'],
    options: {
      image: '84d72a2f82f6',
      Tty: false,
      Detach: true,
      ExposedPorts: { '33000/tcp': {} },
      HostConfig: {
        AutoRemove: true,
        PortBindings: {
          '33000/tcp': [{
            HostIP: '0.0.0.0',
            HostPort: '33100'
          }],
        },
      }
    },
    needsTraining: false,
    description: {
      first: 'The im2txt model, also called the Show and Tell model, is a deep neural network that learns how to describe the content of images',
      second: 'More technically, it is an example of an encoder-decoder neural network. It works by first "encoding" an image into a fixed-length vector representation, and then "decoding" the representation into a natural language description.',
      third: 'The image encoder is a deep convolutional neural network. This type of network is widely used for image tasks and is currently state-of-the-art for object recognition and detection. Our particular choice of network is the Inception v3 image recognition model pretrained on the ILSVRC-2012-CLS image classification dataset. The decoder is a long short-term memory (LSTM) network. This type of network is commonly used for sequence modeling tasks such as language modeling and machine translation. In the Show and Tell model, the LSTM network is trained as a language model conditioned on the image encoding.',
      authors: 'Oriol Vinyals, Alexander Toshev, Samy Bengio, Dumitru Erhan.',
      image: 'https://raw.githubusercontent.com/cvalenzuela/runway_models/master/im2txt/imgs/demo.png?token=AKHU_R2F7AQImVwfzZ1RqeRfVKYWJYwhks5azcGywA%3D%3D',
      organization: 'Google',
      framework: 'Tensorflow',
      github: 'https://github.com/tensorflow/models/tree/master/research/im2txt',
      paper: 'https://arxiv.org/abs/1609.06647',
      licence: 'Apache License 2.0',
    },
    inputs: ['Image', 'Video']
  },
```

## Deploying as stand-alone apps

Since each container is just a server, 

## Contributing

SEE [CONTRIBUTING.md]()
