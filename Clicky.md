# Clicky 

This was a reverse engineering challenge. We were provided with a file called ClickMe.exe. 

Because this was a .exe file my first move was to binwalk it to get a .NET executable I could run through a decompiler. 

```c
binwalk --dd=".*" ClickMe.exe
```

```c
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Microsoft executable, portable (PE)
147279        0x23F4F         XML document, version: "1.0"
148992        0x24600         Microsoft executable, portable (PE)
165652        0x28714         JPEG image data, JFIF standard 1.01
202575        0x3174F         XML document, version: "1.0"
```

The .NET executable will be one of the Microsoft executables PE files. 

Going in to _ClickMe.exe.extracted and checking further by running file * I got: 
```c
0:     PE32+ executable (GUI) x86-64, for MS Windows
23F4F: data
3174F: data
24600: PE32+ executable (GUI) x86-64 Mono/.Net assembly, for MS Windows
28714: JPEG image data, JFIF standard 1.01, resolution (DPI), density 0x0, segment length 16, progressive, precision 8, 400x400, components 3
```
So 24600 was the Mono/.Net assembly file I needed to run through a decompiler, in this case dnSpy. 

One of the first things a came across while reading my decompiled code was this: 

```c
public Form1()
		{
			base.Load += this.Form1_Load;
			this.flag = "ZXc9PQ==";
			this.galf = "ZlE9PQ==";
			this.contents = "bmV2ZXIgZ29ubmEgZ2l2ZSB5b3UgdXA=";
			this.I_am_speedrunner = new Stopwatch();
			this.InitializeComponent();
		}
```
flag, galf, contents yes this seems like I'm in the right place. 

After some easy decoding with cyber chef - 

this.flag = "{"

this.galf = "}" 

this.contents = "never gonna give you up"

So with 1% chance of the flag being a rick alstey reference in mind I kept looking. 

Then I came across 
```c 
MessageBox.Show(string.Concat(new string[]
				{
					"How did you do this?!?! ",
					this.Unscramble("UkZWRFZFWT0="),
					this.Unscramble(this.flag),
					this.Unscramble(this.Random_function(this.Label1.Text)),
					this.Unscramble(this.Random_function(this.Label2.Text)),
					this.Unscramble(this.Random_function(this.Label3.Text)),
					"_ZGVhZGIzM2ZjYWZl",
					this.Unscramble(this.galf)
				}));
```
A very promising return of this.flag and this.galf

label1.Text, label2.Text and label3.Text were not defined too far away.  
```c 
      this.Label1.AutoSize = true;
			this.Label1.Location = new Point(1119, 1184);
			this.Label1.Name = "Label1";
			this.Label1.Size = new Size(336, 25);
			this.Label1.TabIndex = 7;
			this.Label1.Text = "576B64736131677A62485A6B55543039";
			this.Label2.AutoSize = true;
			this.Label2.Location = new Point(1117, 1219);
			this.Label2.Name = "Label2";
			this.Label2.Size = new Size(254, 25);
			this.Label2.TabIndex = 8;
			this.Label2.Text = "57444E57656C70574F44303D";
			this.Label3.AutoSize = true;
			this.Label3.Location = new Point(1115, 1254);
			this.Label3.Name = "Label3";
			this.Label3.Size = new Size(255, 25);
			this.Label3.TabIndex = 9;
			this.Label3.Text = "57565935565646575453383D";
```
I then decoded each section, 


UkZWRFZFWT0= => DUCTF

this.flag => {

this.label1.Text => did_you

this.label2.Text => _use_

this.label3.Text => a_TAS?

"_ZGVhZGIzM2ZjYWZl" did not get unscrambled so it just goes straight into the flag but as an easter egg it is base64 deadb33fcafe. 

this.galf => }

All together we get the flag. 

DUCTF{did_you_use_a_TAS?_ZGVhZGIzM2ZjYWZl}
