This toolsuite will in an intelligent way let you hide data inside the zip structure of zip based files. There are a few similar ways to hide data in the zip structure, but they will trigger error messages in Office when opening such ooXML documents (newstyle office document format which is zip based). I therefore settled on using the Extra Field for injected data, since it works fine on most zip dependant solutions (have not seen a case it does not work in), and MS Office by far the most picky one. Other ooXML files created by OpenOffice for instance, are still zip files, and will thus also work.

Some details:
The Extra Field can exist in both the Local File Headers (LFH) and/or the Central Directory Headers (CDS). The size of it is restricted by a dword (0xFFFF), which equates to roughly 65 KB per Extra Field entry. On a regular zip archive with one file, there would thus be possible to hide roughly 65 * 2 KB. Luckily for ooXML files there are by default quite a lot of files. On a minimal docx that would be about 700 KB, and with a pptx capable of far more. Or a zip archive with n files would be capable of n * 65 Kb. Note that Office solutions using ooXML have certain restrictions on the use of EF in the LFH, which means only certain of those can contain data of limited size. The good thing is that such restrictions don't exist for the CDS. However it is far easier to detect hidden data in the CDS than in LFH. According to the ooXML specification this field can be reserved as much as 65535 bytes per entry (minus the size of the other fields of that entry). In practice only the entries in CDS can hold that much data without Office complaining. The EF in LFH appears to handle a maximum of 256 or 512 bytes, and only for certain of the files. MS Office uses it own signature inside the EF and always starts with "20 A2" and followed by the total size of EF minus 4 bytes. Actually only these first 4 bytes of the header is actually used, leaving the next 4 bytes of the header free, as well as the whole following blocks of either 256 or 512 bytes. To be able to extract any hidden data, there is a custom header created consisting of information about data size, data relative offset. 

The hidden data can be optionally compressed and encrypted. At least compression is recommended. The application also supports keeping timestamps on the modified file. The extracter will save the outfile in the same directory as the extracter is run from, and based on content header an extension is given. Note that if a zip based file is to be injected into another zip file, you must at least compress the file, or else the zip structure identification gets messed up. 

How to use:
1. Start by browsing for the file to be hidden.
2. Optionally tick off for compression. 
3. Optionally tick off for encryption (AES 256) and select password.
3. Click "Prepare secret data".
4. Click browse and select a container (zip, docx etc) where the secret data will be injected.
5. Based on the size of the prepared secret data, method 1 - 4 will be made available or greyed out.
6. Optionally tick off for keeping the filesystem timestamps.
7. In the dropdown box, available archives will be shown. Select one. (no need to remember it as the extracter will auto-detect content).
8. Click "Prepare archive".
9. Lastly click on "Hide file".


Some limitations;
- MS Office / OpenOffice will wipe the extra field if the document is modified and saved (other zip handling software do not behave like this - like WinRar and Windows' own zip handler keeps the hidden data after a mod & save operation).
- Program must be restarted after each hide/unhide (yes I know it's lame).
- When fragmenting with method 4, only 1 use of this method can be used on the same container.
- And obviously size of payload is restricted by the number of files inside the zip.

To detect this kind of stuff, you really have to manually and carefully inspect the zip file.


The original thread with more details and descriptions can be found here; http://www.forensicfocus.com/index.php?name=Forums&file=viewtopic&t=7918

Discussions can follow here; 


Changelog:
v4
Added x64 compiled exe's.

Hider:
- Deactivated XOR option, since it was incorrect and either way probably not needed.
- Fixed a tiny error in the timestamp handling that crashed the app when run from inside a x64 OS.

UnHider:
- Deactivated XOR option.
- Added a few filesignatures for the detection of the extracted content.

