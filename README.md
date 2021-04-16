# ImgSqizer

## ImgSqizer (Pronounced Image squeezer) is a simple image compressor

### usage:
simply drag and drop images you want to compress over ImgSqizer.exe

the application will create an image with the same name and adds "compressed" suffix

**Note**: it is best used for JPEG images, PNG can sometimes create larger image in size, so it is hit or miss.

----

** which one should i download? **

 choose your operating system then you can choose either WR or Light file.
 the difference is WR (With Runtime) is bundled with .NET 5 runtime while Light isn't. 
 if you dont have .net 5 runtime or newer you can't run light version.
 WR is bigger in size compared to light
|App|WR|Light|
|----|----|----|
|win|20830 KB|827 KB|
|Linux|22131 KB|681 KB|
|osx|22946 KB|639KB KB|

you can download the .net 5 runtime at [here](https://dotnet.microsoft.com/)



### code: 
for all our lazy friends :)

**Note** all aplication is in x64 if you wish to change it follow those steps:
- download the project
- navigate to ImgSqizr folder
- run this command `dotnet publish -r osx-x64 -c Release /p:PublishSingleFile=true /p:IncludeNativeLibrariesForSelfExtract=true --self-contained false` and change osx-x64 to your liking 
- you can find list of support arguments [here](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog)

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
