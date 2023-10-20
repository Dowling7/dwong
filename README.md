# dwong, a package for DarkQuest data analysis.

dwong is a comprehensive Python package, created by student Dowling Wong, tailored for data analysis and neural network-based particle identification in the DarkQuest experiment. The aim of this project is to streamline DarkQuest's data analysis process by providing exemplary data-processing functions.

The package mainly contains four modules: dwong, dplot, dcsv and dkeras.
## Contents and useful functions.
* dwong
  * emcal_bytuple
  * multi_clusters
  * h4_bytuple
  * prepare_data_bytuple
* dplot
  * emcal_evt(x, y, eng)
  * emcal_pdf(ntuple_name, fname, absolute_path)
* dkeras
  * train_model(x, y)
  * save_model(model, fname)
  * load_model(mname)
  * plot_confusion_matrix(cm, names, title='Confusion matrix', cmap=plt.cm.Blues)
  * plot_roc(pred,y)
* dcsv
  * gen_csv(filename)

  
![scheme](/logo/darkquest_schematic.png " inline image")
## Smaple of use.

### dwong, main module for data analysis.
<pre>
 import dwong
 
 dq_events = dwong.getData(filename, "Events") #data acquisition from n-tuple.
 (x, y, eng, labels, labels_decrease, seeds, seed_labels) = dwong.multi_clusters(dq_events)#here performed clustering
 (h4x, h4y) = h4_bytuple(dq_events)
 dq_st23 = dq_events["st23"]
 dq_track = dq_events["track"]
 gpz = dq_events["gen"]["pz"]
 trkls_coord = np.stack((dq_st23["x"], dq_st23["y"], dq_st23["z"], dq_st23["px"], dq_st23["py"], dq_st23["pz"]), axis=1)
 trkls_cal = np.stack((dq_st23["Cal_x"], dq_st23["Cal_y"]), axis=1)
 track_st3 = np.stack((dq_track["x"], dq_track["y"], dq_track["pz"]), axis=1)

 folded_list=dwong.prepare_data_bytuple(filename)#return a list of events, each event may contain multiple particles.   
 flat_list = [particle for event in folded_list for particle in event]
 labels = [0] * len(flat_list)
 labeled_flat_list = [[label, *particle] for label, particle in zip(labels, flat_list)]#list of particles, in a flat list.
</pre>

### dplot, plot module.
<pre>
 import dwong
 from dwong import dplot

 dq_events = dwong.getData(filename, "Events") #data acquisition from n-tuple.
 (x, y, eng) = emcal_bytuple(dq_events)
 fig = emcal_evt(x, y, eng)
 #The emcal_evt will plot the emcal page in jupyter notebook, and if you have further consideration, it returns fig

 #save a pdf booklet for all the emcal plots for events in a root file
 if ntuple_name.endswith(".root")& (ntuple_name not in train):
        emcal_pdf(ntuple_name, fname, absolute_path)

 #or you can plot all n-tuple under a directory
 target_dir = os.listdir("/Users/dwong/Desktop/n-tuples/5_80_training/")
 for file_name in target_dir:
     os.chdir(taregt_dir)
     if file_name.endswith(".root")& (file_name not in train):
         emcal_pdf(ntuple_name, fname, absolute_path)
 
</pre>

## External Link
*[DarkQuest Snowmass paper][snowmass].
*[DarkQuest Collaboration code collection][DQ_collab].
*[The source for this project is available here][src].
*[Dowling's code collection for data analysis, model training, particle ID and samples][DQ_Dowling].
*[Analysis package dwong's Pypi page][dwong_pypi].
*[Dowling's personal website][personal_website].


The metadata for a Python project is defined in the `pyproject.toml` file,
an example of which is included in this project. You should edit this file
accordingly to adapt this sample project to your needs.

----

This is the README file for the project.

The file should use UTF-8 encoding and can be written using
[reStructuredText][rst] or [markdown][md use] with the appropriate [key set][md
use]. It will be used to generate the project webpage on PyPI and will be
displayed as the project homepage on common code-hosting services, and should be
written for that purpose.

Typical contents for this file would include an overview of the project, basic
usage examples, etc. Generally, including the project changelog in here is not a
good idea, although a simple “What's New” section for the most recent version
may be appropriate.

[snowmass]: https://arxiv.org/abs/2203.08322
[DQ_collab]: https://github.com/DarkQuest-FNAL
[DQ_Dowling]: https://github.com/Dowling7/DQ_Dowling
[src]: https://github.com/Dowling7/dwong/tree/main/src
[dwong_pypi]: https://pypi.org/project/dwong/
[personal_website]: https://dowling7.github.io/my_website/
