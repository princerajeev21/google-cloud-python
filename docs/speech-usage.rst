Using the API
=============

The `Google Speech`_ API enables developers to convert audio to text.
The API recognizes over 80 languages and variants, to support your global user
base.

.. warning::

   This is a Beta release of Google Speech API. This
   API is not intended for real-time usage in critical applications.

.. _Google Speech: https://cloud.google.com/speech/docs/getting-started

Client
------

:class:`~google.cloud.speech.client.Client` objects provide a
means to configure your application. Each instance holds
an authenticated connection to the Natural Language service.

For an overview of authentication in ``google-cloud-python``, see
:doc:`google-cloud-auth`.

Assuming your environment is set up as described in that document,
create an instance of :class:`~google.cloud.speech.client.Client`.

  .. code-block:: python

     >>> from google.cloud import speech
     >>> client = speech.Client()


Asychronous Recognition
-----------------------

The :meth:`~google.cloud.speech.Client.async_recognize` sends audio data to the
Speech API and initiates a Long Running Operation. Using this operation, you
can periodically poll for recognition results. Use asynchronous requests for
audio data of any duration up to 80 minutes.

See: `Speech Asynchronous Recognize`_


  .. code-block:: python

      >>> import time
      >>> operation = client.async_recognize(
      ...     None, 'gs://my-bucket/recording.flac',
      ...     'FLAC', 16000, max_alternatives=2)
      >>> retry_count = 100
      >>> while retry_count > 0 and not operation.complete:
      ...     retry_count -= 1
      ...     time.sleep(10)
      ...     operation.poll()  # API call
      >>> operation.complete
      True
      >>> operation.results[0].transcript
      'how old is the Brooklyn Bridge'
      >>> operation.results[0].confidence
      0.98267895


Synchronous Recognition
-----------------------

The :meth:`~google.cloud.speech.Client.sync_recognize` method converts speech
data to text and returns alternative text transcriptons.

  .. code-block:: python

     >>> alternatives = client.sync_recognize(
     ...     None, 'gs://my-bucket/recording.flac',
     ...     'FLAC', 16000, max_alternatives=2)
     >>> for alternative in alternatives:
     ...     print('=' * 20)
     ...     print('transcript: ' + alternative['transcript'])
     ...     print('confidence: ' + alternative['confidence'])
     ====================
     transcript: Hello, this is a test
     confidence: 0.81
     ====================
     transcript: Hello, this is one test
     confidence: 0

.. _sync_recognize: https://cloud.google.com/speech/reference/rest/v1beta1/speech/syncrecognize
.. _Speech Asynchronous Recognize: https://cloud.google.com/speech/reference/rest/v1beta1/speech/asyncrecognize