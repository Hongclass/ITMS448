These two tools will hide and extract data hidden into the digital signature of ooXML documents (docx, xlsx, pptx etc). The point is to prove that the signatures protect only parts of a document. Among others, [Content_Types].xml is unprotected, as is sig1.xml (the signature file). For example document metadata is completely unprotected. In addition, the signing is performed before the final packaging (zipping), which also opens for further modifications to the zip structure. But the way the signing is currently implemented, there is no way to protect the zip structure.

The data hider, ooXMLSignatureTweaker.exe, will compress the secret data and then apply base64 encoding to it, before putting it inside sig1.xml. Currently it will use default xml tags to hide in and can be located like this;

</Object>
<X509Data>
<X509Certificate>88xLSU1N0VEoycgsVgCiRIWy1KJKheLU5KLUEoXc1OLixPRUPT0A</X509Certificate> 
</X509Data>
</Signature>

Although the injected tags are in fact valid tags, they are located outside the actual signature and therefore not evaluated. If you scroll up a bit in your signature, you will see that the tags are already present.. Actually there are many places inside sig1.xml you can put stuff like this, and you can use entirely custom-named tags too. Have successfully injected 6 Mb of data in the current version. Because the zip handling is done internally, without dependency on external libraries, we have more control over how the reassembling of the document is done. For that reason, it may be noticed that the zip timestamps are not touched. That means the only way of detecting this kind of hidden data is by carefully evaluating all the XML tags.

The extracter part, ooXMLSignatureRipper.exe, will simply locate the hidden data and extract it to a named file. 

The trick works on Office 2007 and 2010. Have not tried other ooXML capable applications. There are both x86 and x64 binaries included that are tested to work OK.

Changelog:
v1002
- Fixed bug in identification of non-signed documents.
- Fixed some error codes.
