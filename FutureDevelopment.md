# FUTURE DEVELOPMENT #

To discuss future development,  some ideas are summarized and organized below.


---

# 1. Make stf and stfio modules more pythonic #

Replace getter/setter functions with Python properties; e.g. we should replace

```
start_cursor = stf.get_fit_start()
stf.set_fit_start(start_cursor + 20)
end_cursor = stf.get_fit_end()
stf.set_fit_end(end_cursor + 20)
```

with

```
start_cursor = stf.cursors.fit[0]
stf.cursors.fit[0] = start_cursor + 20
end_cursor = stf.cursors.fit[1]
stf.cursors.fit[1] = end_cursor + 20
```

# 2. Data file formats #

> ## ATF export/import ##
Exported ATF files can not be imported in Stimfit.
I tried to move the ATF import into biosig.  But because of that issue,
I'm not sure which is the proper ATF specification. Either exportATF, or
importATF, or both does not work as it should. I do not know what the
"proper" ATF looks like, does anyone of you know ?

> ## move native file handling to BioSig ##
BioSig is available in [http:://biosig.sf.net libbiosig].
There are 2 requirements for this:
> > First, all file formats need to be supported by biosig, and second, we'd need to get biosig into MacPorts so that it's available on all platforms.
> > support for the following formats

  * ABF2: initial support in biosig - but far from complete
> > need example files for testing
  * ATF: no excact specification available see also #ATF
> > need example files for testing (not from Stimfit's exportATF)
  * AXG: some support in biosig - incomplete
> > need example files for testing
  * old HEKA Pulse file: does not open ([issue 32](https://code.google.com/p/stimfit/issues/detail?id=32))
  * SON/SMR: initial support in biosig - but far from complete

  * WinWCP: no started: need of example files and/or specification
> > need example files for testing
  * HDF5: not sure whether this should really go in biosig


> ## BioSig port to MacPorts ##

> for the reason above, BioSig hast to be ported to MacPorts

> ## HDF5, GDF ##
Would it be possible to represent GDF structures on top of the hdf5 format? That way these files could be read by MATLAB, Python etc. without any additional modules    (e.g. http://www.mathworks.co.uk/help/matlab/ref/h5read.html)

No, that's not the way to go; HDF5 is  a "container format"; you can put all kind of data in HDF5, but it does not specify how to do it. So, everyone can put the same data in different ways into the container. This is not good enough to provide interoperability. I know also other biosigal software that uses HDF5 to store biosignal data into Stimfit, I'm pretty sure that data can not be opened with Stimfit and vice versa. Going to HDF5 shifts the problem to the consuming software - each software would need to import filters for all variants of HDF5. If we have now about 80 different biosignal formats, moving to hdf5 would result in 80 different, hdf-based file formats - there is nothing gained here.

The advantage of GDF is that there is no (or very little) room for ambiguity, there is just one way how data has to be stored. This strictness is an advantage when it comes to compatibility. GDF has been designed with the idea "to combine the best features of all biosignal data formats in a single format".

> ## Neo's shared object model ##
> Adopt neo's shared object model for better data exchange.


> ## Support WinWCP files ##
> (http://spider.science.strath.ac.uk/sipbs/software_ses.htm). I've had a couple of requests and WinWCP seems to be getting increasingly popular. Maybe this is interesting for BioSig?

> Certainly, do you have or can you get some example files ?


---


# 3. Visualization #
> ## y-scale related wrap-around ##

If the y-scaling of a trace becomes larger and larger, eventually
there is a wrap-around of the traces. Values larger than some maximum-integers become negative and values smaller than some minimum integer become positive, resulting in
additional vertical lines when plotting the traces.

> ## matplotlib export ##
The matplotlib figure export is far from perfect and needs to be optimized so that figures can directly be used for presentations etc. It would be good to include options such as plotting cursors and results, and to export the Python source if required.


---


# 4. Algorithms and general handling #

> ## Move Math-funtions to libstfio ##
Move all math functions (fitting and template matching in particular) to libstfio so that they can be used without the stimfit GUI, and rename libstfio, because on the long run we want to use only libbiosig for file I/O.

I'd rather suggest keeping libstfio just for import/export, a make a separate libstf (similar to libstfio) for all math functions. So that it can be used without the stimfit GUI.

CSH Good idea. Maybe we can call this separate library "libstfnum" - "math" doesn't quite capture the numerical algorithms. On the long run, libstfio will just be an adapter library for libbiosig.

We should also go further in conceptually dividing the GUI from data handling and algorithms. At this time, there's too much non-GUI stuff in doc.cpp for example. It would be good to have a non-GUI dependent concatenation function in recording.cpp so that we can access it from Python.

> ## cursor positioning error messages ##
The cursor positioning error messages are unnerving. For instance, we should just silently swap cursors if they are reversed rather than showing an alert dialog.

This is the easy case, but what should we do when the sweeps have different length? E.g. if there short sweeps (e.g. only the 1/10 length) and you want to skim through. E.g. there are long (L) and short (S) sweeps mixed, but the users is mostly interested in L sweeps

> LLLLLLLSLLLLLLSLLLLLLLSLLLLLLSLLLLLLLSLLLLLLSLLLLLLLSLLLLLLS

should the cursor positions be changed when reaching the S sweeps  or not ?
I guess not, but then the on-the-fly analysis (measure.cpp) must be made exception free and return "undefined", or "not available" using NaN as return values.

opened [issue 39](https://code.google.com/p/stimfit/issues/detail?id=39) on this topic

> ## batch processing: some output parameters missing ##
> crossing time of slope threshold
> reviewing of other parameters

- peak time,
- time of maximum rise and
- time of maximum decay
have been added

> see also [issue 14](https://code.google.com/p/stimfit/issues/detail?id=14)

> ## event onset time: different methods for different channels ##

the issue came up with a 3 channel recording, trying to get the latency.
> In one channel the event onset should be identified by the slope
> threshold crossing, in the the other channel with baseline method. Its
> possible to do with the cursor settings dialog but its complicated to do
> (many clicks in the cursor settings dialog). Without Jose's help, we
> would not have managed to do it at all. This comes down to the question
> how to improve the UI and make it more intuitive.

> see also [issue 14](https://code.google.com/p/stimfit/issues/detail?id=14).

> ## compute latency between channels ##

> Isn't this already implemented?

> That's somewhat related to the issue above "event onset time ...",
> perhaps its also an issue when multiple (more than 2) channels are
> available. I do not know what a good solution might be, but I see that
> users have difficulties to find the proper settings for their analysis.

> So far my idea about that developed into this: extend the results table
> by not only providing duration (e.g. half amplitude duration, rise time)
> but also the start and end times - possibly extending this also to
> multiple channels. Of course the user would also need a way for
> specifying which time differences are desired.

> Perhaps this is also related to [issue 14](https://code.google.com/p/stimfit/issues/detail?id=14) "batch analysis across multiple
> channels"

> ## Add support for physical quantities ##
> Automatically convert SI units (micro, milli, kilo, mega, etc.)


---


# 5. Usability #

> ## built against Enthought Python ##
It would be nice to have versions built against Enthought Python and Python(x,y) on Windows. Hardly anyone uses stock Python for scientific purposes so that most users can't import their default scientific modules. The most elegant solution would be to have the users select their Python environment from the program, but this will be difficult because of Python library incompatibilities.

> ## 32 bit limitation of windows bundle ##
Large (Heka) files can not be opened on 64bit Windows -
> > because stimfit bundle with python is still 32 bit

We'd just have to build for 64 bits with MSVC2008. The only issue is that this is time consuming because we'd need to rebuild all dependencies for 64 bits as well. And we'd need a full version of MSVC2008 because the Express version only supports 32 bits.


> ## direct link to Spreadsheet application (Excel,OOCalc) ##
> What exactly do you mean? A button to open the default spreadsheed application?

> in some axon software (pClamp?), there is a way to move data from the
> results table (e.g. columns) to excel using just some right mouse click.
> at least one user likes that feature and was asking whether stimfit
> could do something like this.

> ## storing and retrieving of different cursor settings in external files ##

(Jose) My idea was to implement an option that allows the user to load/save all options about cursors. For example we would save all cursor positions, but also peak direction or threshold value, etc. This would allow the users to save different sessions of analysis of a file and use it (or see it) later. I guess this is something similar to the menu -> View -> Save window position and View-> Load window position (which is not working, by the way).

> ## stimfit without python ##

figure generation should work, candidate engines are
> [wxMathPlot](http://wxmathplot.sourceforge.net/)
> [gnuplot](http://www.gnuplot.info/)




# Issues #

> ## warning (on windows only) when opening some files ##
> commit fefc125946717605c2b5a0b95c gets rid of this warning,
> but the patch need to be reviewed whether its the right way to do.

> ## [issue 13](https://code.google.com/p/stimfit/issues/detail?id=13): concatenate multi-channel files ##
> implemented - but review needed
> +1

> ## [issue 14](https://code.google.com/p/stimfit/issues/detail?id=14): Batch analysis across multiple channels ##
> +1

> ## [issue 21](https://code.google.com/p/stimfit/issues/detail?id=21): Stimfit will not work with IPython versions 0.11 ##
> or above. It would be great to have that given that IPython is the default Python shell for scientific purposes.

> ## [issue 36](https://code.google.com/p/stimfit/issues/detail?id=36): menu toolbar shortcuts do not work on windows ##

> Couldn't reproduce on Linux. The problem can be reproduced on Windows;
> the windows bundle as well as the MXE build show this problem.