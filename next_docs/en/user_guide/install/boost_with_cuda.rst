
Boost With Cuda 
================


If your device supports CUDA and meets the GPU requirements of the
mainline environment, you can use GPU acceleration. Please select the
appropriate guide based on your system:

-  :ref:`ubuntu_22_04_lts_section`
-  :ref:`windows_10_or_11_section`


.. _ubuntu_22_04_lts_section:

Ubuntu 22.04 LTS
-----------------

1. Check if NVIDIA Drivers Are Installed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

   nvidia-smi

If you see information similar to the following, it means that the
NVIDIA drivers are already installed, and you can skip Step 2.

.. note::

   ``CUDA Version`` should be >= 12.4, If the displayed version number is less than 12.4, please upgrade the driver.

.. code:: text

   +---------------------------------------------------------------------------------------+
   | NVIDIA-SMI 570.133.07             Driver Version: 572.83         CUDA Version: 12.8   |
   |-----------------------------------------+----------------------+----------------------+
   | GPU  Name                     TCC/WDDM  | Bus-Id        Disp.A | Volatile Uncorr. ECC |
   | Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
   |                                         |                      |               MIG M. |
   |=========================================+======================+======================|
   |   0  NVIDIA GeForce RTX 3060 Ti   WDDM  | 00000000:01:00.0  On |                  N/A |
   |  0%   51C    P8              12W / 200W |   1489MiB /  8192MiB |      5%      Default |
   |                                         |                      |                  N/A |
   +-----------------------------------------+----------------------+----------------------+

2. Install the Driver
~~~~~~~~~~~~~~~~~~~~~

If no driver is installed, use the following command:

.. code:: sh

   sudo apt-get update
   sudo apt-get install nvidia-driver-570-server

Install the proprietary driver and restart your computer after
installation.

.. code:: sh

   reboot

3. Install Anaconda
~~~~~~~~~~~~~~~~~~~

If Anaconda is already installed, skip this step.

.. code:: sh

   wget https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Linux-x86_64.sh
   bash Anaconda3-2024.06-1-Linux-x86_64.sh

In the final step, enter ``yes``, close the terminal, and reopen it.

4. Create an Environment Using Conda
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specify Python version 3.10～3.13.

.. code:: sh

    conda create -n mineru 'python=3.12' -y
    conda activate mineru

5. Install Applications
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

   pip install -U magic-pdf[full]

.. admonition:: TIP
    :class: tip

    After installation, you can check the version of ``magic-pdf`` using the following command:

.. code:: sh

   magic-pdf --version


6. Download Models
~~~~~~~~~~~~~~~~~~

Refer to detailed instructions on :doc:`download_model_weight_files`

7. Understand the Location of the Configuration File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After completing the `6. Download Models <#6-download-models>`__ step,
the script will automatically generate a ``magic-pdf.json`` file in the
user directory and configure the default model path. You can find the
``magic-pdf.json`` file in your user directory.

.. admonition:: TIP
    :class: tip

    The user directory for Linux is “/home/username”.

8. First Run
~~~~~~~~~~~~

Download a sample file from the repository and test it.

.. code:: sh

   wget https://github.com/opendatalab/MinerU/raw/master/demo/pdfs/small_ocr.pdf
   magic-pdf -p small_ocr.pdf -o ./output

9. Test CUDA Acceleration
~~~~~~~~~~~~~~~~~~~~~~~~~

If your graphics card has at least **8GB** of VRAM, follow these steps
to test CUDA acceleration:

1. Modify the value of ``"device-mode"`` in the ``magic-pdf.json``
   configuration file located in your home directory.

   .. code:: json

      {
        "device-mode": "cuda"
      }

2. Test CUDA acceleration with the following command:

   .. code:: sh

      magic-pdf -p small_ocr.pdf -o ./output


.. _windows_10_or_11_section:

Windows 10/11
--------------

1. Install CUDA
~~~~~~~~~~~~~~~~~~~~~~~~~

You need to install a CUDA version that is compatible with torch's requirements. For details, please refer to the [official PyTorch website](https://pytorch.org/get-started/locally/).

- CUDA 11.8 https://developer.nvidia.com/cuda-11-8-0-download-archive
- CUDA 12.4 https://developer.nvidia.com/cuda-12-4-0-download-archive
- CUDA 12.6 https://developer.nvidia.com/cuda-12-6-0-download-archive
- CUDA 12.8 https://developer.nvidia.com/cuda-12-8-0-download-archive


2. Install Anaconda
~~~~~~~~~~~~~~~~~~~

If Anaconda is already installed, you can skip this step.

Download link: https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Windows-x86_64.exe

3. Create an Environment Using Conda
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    conda create -n mineru 'python=3.12' -y
    conda activate mineru

4. Install Applications
~~~~~~~~~~~~~~~~~~~~~~~

::

   pip install -U magic-pdf[full]

.. admonition:: Tip
    :class: tip

    After installation, you can check the version of ``magic-pdf``:

    .. code:: bash

      magic-pdf --version


5. Download Models
~~~~~~~~~~~~~~~~~~

Refer to detailed instructions on :doc:`download_model_weight_files`

6. Understand the Location of the Configuration File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After completing the `5. Download Models <#5-download-models>`__ step,
the script will automatically generate a ``magic-pdf.json`` file in the
user directory and configure the default model path. You can find the
``magic-pdf.json`` file in your 【user directory】 .

.. admonition:: Tip
    :class: tip

    The user directory for Windows is “C:/Users/username”.

7. First Run
~~~~~~~~~~~~

Download a sample file from the repository and test it.

.. code:: powershell

     wget https://github.com/opendatalab/MinerU/raw/master/demo/pdfs/small_ocr.pdf -O small_ocr.pdf
     magic-pdf -p small_ocr.pdf -o ./output

8. Test CUDA Acceleration
~~~~~~~~~~~~~~~~~~~~~~~~~

If your graphics card has at least 8GB of VRAM, follow these steps to
test CUDA-accelerated parsing performance.

1. **Overwrite the installation of torch and torchvision** supporting CUDA.(Please select the appropriate index-url based on your CUDA version. For more details, refer to the [PyTorch official website](https://pytorch.org/get-started/locally/).)

.. code:: sh

   pip install --force-reinstall torch torchvision --index-url https://download.pytorch.org/whl/cu124


2. **Modify the value of ``"device-mode"``** in the ``magic-pdf.json``
   configuration file located in your user directory.

   .. code:: json

      {
        "device-mode": "cuda"
      }

3. **Run the following command to test CUDA acceleration**:

   ::

      magic-pdf -p small_ocr.pdf -o ./output
