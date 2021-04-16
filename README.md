# ImgSqizer

## ImgSqizer (Pronounced Image squeezer) is a simple image compressor

### usage:
simply drag and drop images you want to compress over ImgSqizer.exe

the application will create an image with the same name and adds "compressed" suffix

**Note**: it is best used for JPEG images, PNG can sometimes create larger image in size, so it is hit or miss.

### code: 
for all our lazy friends :)

```csharp

        static void Main(string[] args)
        {
            if (args.Length!=0)
            {
                for (int i = 0; i < args.Length; i++)
                {
                    var filePath = args[i];
                    var extension =Path.GetExtension(filePath).ToUpper();
                    var output =Path.Combine(Path.GetDirectoryName(filePath),Path.GetFileNameWithoutExtension(filePath)+" Compressed"+Path.GetExtension(filePath));
                    if (!File.Exists(output) && (extension== ".JPG" || extension == ".PNG") )
                    {
                        CompressImage(filePath, 60, output);
                    }
                }
            }
        }
        private static void CompressImage(string InputImage, int Quality, string OutPutDirectory)
        {
            using (Bitmap mybitmab = new Bitmap(@InputImage))
            {
                ImageCodecInfo imageEncoder = GetEncoder(ImageFormat.Jpeg);
                Encoder myEncoder = Encoder.Quality;
                EncoderParameters myEncoderParameters = new EncoderParameters(1);
                EncoderParameter myEncoderParameter = new EncoderParameter(myEncoder, Quality);
                myEncoderParameters.Param[0] = myEncoderParameter;
                mybitmab.Save(OutPutDirectory, imageEncoder, myEncoderParameters);
            }

        }
        private static ImageCodecInfo GetEncoder(ImageFormat format)
        {
            ImageCodecInfo[] codecs = ImageCodecInfo.GetImageDecoders();
            foreach (ImageCodecInfo codec in codecs)
            {
                if (codec.FormatID == format.Guid)
                {
                    return codec;
                }
            }
            return null;
        }
```
