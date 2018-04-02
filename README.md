# CSharpHastebinAPI
A small method for uploading text to hastebin.com


# Code
```c#
using System;
using System.Net;
using System.Runtime.Serialization.Json;
using System.Text;
using System.Xml;
using System.Xml.Linq;
using System.Xml.XPath;

string UploadHaste(string text)
{
    string returnedjson = null;

    using (WebClient client = new WebClient())
        returnedjson = client.UploadString("https://hastebin.com/documents", text);

    if (returnedjson == null)
        throw new Exception("Failed to get response from https://hastebin.com/documents");

    using (XmlDictionaryReader jsonReader =
        JsonReaderWriterFactory.CreateJsonReader(Encoding.UTF8.GetBytes(returnedjson),
            new System.Xml.XmlDictionaryReaderQuotas()))
    {
        return $"hastebin.com/{XElement.Load(jsonReader).XPathSelectElement("//key")?.Value}";
    }
}
```

# Example Calling

```c#
void SomeMethod() {
  Console.WriteLine(UploadHaste("some example test"));
  //will print a hastebin link containing your text!
}
```
