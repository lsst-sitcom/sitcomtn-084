:tocdepth: 1

.. sectnum::

.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

Abstract
========

This is the technote for the Position Repeatability Analysis on the TMA with M1M3. We measured the mirror motion (position and rotation) before and after a slew. 
Analysis was done for slews reaching different elevation angles, and also for different slewing speed. 

Related SITCOM tickets
======================

SITCOM - 797 : M1M3 - Slewing analysis - Positioning

SITCOM - 730 : Create data analysis script/notebook for LVV-T235

SITCOM - 854 : Update LVV-T235 notebook

SITCOM - 810 : Create data analysis script/notebook for LVV-T235 - Raise/Park Repeatability


SITCOM - 797 : M1M3 - Slewing analysis - Positioning
====================================================

For this analysis, we compare the positions and rotations reported by the IMS before and after a slew. The analysis is done using different elevation angles and speed.

The initial requirements are the following:

- the mirror decentering (x and y) is less than +/- 6 micrometers
- the mirror tilt is less than +/- 24e-6 degrees (0.0864 arcsec)
- piston (z displacement) is less than +/- 1 micrometer

.. image:: _static/797_histogram_position_rotation_xyz.png
  :width: 700px

Figure 1. Histogram of position and rotation difference after - before slew. The green lines indicate the tolerance intervals 

.. image:: _static/797_position_rotation_xyz.png
  :width: 700px

Figure 2. x, y, z position and rotation shifts as a function of the elevation angle difference. The color scale shows the elevation speed and the green lines indicate the tolerance intervals. 

.. image:: _static/Position_Data_IMS_20230711T185330.png
  :width: 700px

Figure 3. Mirror position as reported by the IMS during a large (74 degrees) slow (0.5 degrees/second) elevation slew.  The black dotted lines are the spec from SITCOM-797.

From these plots we conclude that the initial specifications are not met, especially for the piston (z displacement). After discussion, it was realized that the displacements are normal and correspond to the sag of the mirror cell due to gravity change and will be compensated by adjusting the M2 and camera hexapods. 

SITCOM - 797 : M1M3 - Compensating mirror motion with the hexapods.
===================================================================

The strategy to compensate the mirror cell displacement by adjusting the M2 and camera hexapods requires that the mirror motion be reproducible, so that a look-up table for the hexapods will be able to follow the motion to the specified tolerances.  So the next step was to look at a large number of slews taken during a random walk session and look at the mirror position as a function of elevation for each of the slews.  This is shown in Figure 4.  The colors are the slew speed as a percentage of maximum speed. The curves seem to group themselves according to the slew speeds.  However, note the speed grouping is ordered as 30-40-50-20, which is the order that the tests were run in.  So it is more likely that the grouping reflects something else varying during the night.  One possibility is that the variation is caused by temperature.  Figure 5 shows the Z-position variation through the same group of slews. Even though the temperature change during the run was only 0.6C, compensating for temperature significantly reduces the scatter in the points. However, note that we are still a factor of 3-5 away from meeting the specification.  More testing is needed to see if we can reduce the scatter to the required level.

Plots 3, 4, and 5 were made with the following notebook:

https://github.com/lsst-sitcom/notebooks_vandv
at:
notebooks/tel_and_site/subsys_req_ver/m1m3/SITCOM-797_MTM1M3_Position_Stability_17Nov23.ipynb

.. image:: _static/Final_Mirror_Position_AzLimits_3_03Aug23.png
  :width: 700px

Figure 4. Mirror position as reported by the IMS at the end of many slews during a random walk session. Detailed explanation in the text.    


.. image:: _static/Mirror_Position_Temperature_03Aug23.png
  :width: 700px

Figure 5. Mirror Z-position as reported by the IMS at the end of many slews during a random walk session. The temperature during the night is shown in the lower left.  Compensating for temperature significantly reduces the scatter in the points, but we are still a factor of 3-5 away from meeting the specification.


SITCOM - 854 : Update LVV-T235 notebook
========================================

Assume that the systematic offset seen in each of the Rotation panels is a calibration issue, and remove the median value from each panel to assess the scatter about the median.

.. image:: _static/854_rotation_sub_median.png
  :width: 700px

Figure 6. Position errors and rotation - median 

The requirement specifies that the rms repeatability of the mirror positioning is what we need. Plot the rms for each of the distributions shown in the violin plot:

.. image:: _static/854_rms_repeatability.png
  :width: 700px

Figure 7. rms repeatability

SITCOM - 810 : Create data analysis script/notebook for LVV-T235 - Raise/Park Repeatability
============================================================================================

In this analysis we use the data from the LVV-T235 test case where the mirror cell is repeatedly lowered and raised. We fit a line to the final raised position as measured by the IMS in order to verify that the slope is compatible with 0 mening that the mirror cell has reached a stable position and is not moving anymore.

The following plot shows the x and y positions (shifted in order to be centered around 0) as a function of time, the vertical lines indicate the various M1M3 "detailedStates". We see that the same pattern is repeated several times over the chosen data taking period.

.. image:: _static/810_overview_ref_subtracted.png
  :width: 700px

If we isolate one of the patterns we get the following figure where the lines show the Hard Point states

.. image:: _static/810_singleloop.png
  :width: 700px

We are interested in the slope of the x and y positions on the flat tops / bottoms just before reaching the state: "HP in Standby". We choose to select and analyze one second of data before "HP in standby".

.. image:: _static/810_slope_plot.png
  :width: 700px

Figure 5. x position between successive ACTIVEENGINEERING and LOWERINGENGINEERING states, and between selected timestamps where all 6 Hard Points are in standby `(motionState == 0)`. 

.. image:: _static/810_position_stability.png
  :width: 700px


We repeat this selection for every cycle in x and y and get a set of plots like the following showing the x and y positions as a function of time in the one-second time windows.


The slope measurement is performed on every subset of data. Statistics are summarized in the following table

.. image:: _static/810_table_slopes.png
  :width: 700px

We conclude that on this dataset, the slopes of x and y positions as a function of time are all compatible with 0 

Reference links
=================
<http://lsst-ts/ts_m1m3supporttesting/M13T012.py>


SITCOM - 797 : M1M3 - Compensating mirror motion with the hexapods.
=======================================================================

 After we got the initial data of the mirror motion as a function of elevation, it was soon clear that we are very far from meeting the specifications given in SITCOM-797.  For example, Figure 7 shows the mirror motion during a large elevation slew.  Note that this is a slow slew at 0.5 degrees/sec., so inertial compensation is less of an issue.  After discussing this, it was realized that this motion is just the sag of the mirror cell due to gravity changes, and it will be compensated by adjusting the M2 and camera hexapods to follow this motion.

.. image:: _static/Position_Data_IMS_20230711T185330.png
  :width: 700px

Figure 7. Mirror position as reported by the IMS during a large (74 degree) slow (0.5 degrees/second) elevation slew.  The black dotted lines are the spec from SITCOM-797.  We are very far from meeting the specifications.


This strategy requires that the mirror motion be reproducible, so that a look-up table for the hexapods will be able to follow the motion to the specified tolerances.  So the next step was to look at a large number of slews taken during a random walk session and look at the mirror position as a function of elevation for each of the slews.  This is shown in Figure 8.  The colors are the slew speed as a percentage of maximum speed. The curves seem to group themselves according to the slew speeds.  However, note the speed grouping is ordered as 30-40-50-20, which is the order that the tests were run in.  So it is more likely that the grouping reflects something else varying during the night.  One possibility is that the variation is caused by temperature.  Figure 8 shows the Z-position variation through the same group of slews. Even though the temperature change during the run was only 0.6C, compensating for temperature significantly reduces the scatter in the points. However, note that we are still a factor of 3-5 away from meeting the specification.  More testing is needed to see if we can reduce the scatter to the required level.


.. image:: _static/Final_Mirror_Position_AzLimits_3_03Aug23.png
  :width: 700px

Figure 7. Mirror position as reported by the IMS at the end of many slews during a random walk session. Detailed explanation in the text.    


.. image:: _static/Mirror_Position_Temperature_03Aug23.png
  :width: 700px

Figure 8. Mirror Z-position as reported by the IMS at the end of many slews during a random walk session. The temperature during the night is shown in the lower left.  Compensating for temperature significantly reduces the scatter in the points, but we are still a factor of 3-5 away from meeting the specification.


.. See the `reStructuredText Style Guide <https://developer.lsst.io/restructuredtext/style.html>`__ to learn how to create sections, links, images, tables, equations, and more.

.. Make in-text citations with: :cite:`bibkey`.
.. Uncomment to use citations
.. .. rubric:: References
.. 
.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa

Acronyms
=========
IMS (Independent measuring system)

HP (Hard Point)

RMS (Root Mean Square)
