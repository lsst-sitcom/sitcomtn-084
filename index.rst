:tocdepth: 1

.. sectnum::

.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

.. note::

   **This technote is a work-in-progress.**

Abstract
========

This is the technote for the Position Repeatability Analysis on the TMA with M1M3 

Related SITCOM tickets
======================

SITCOM - 797 : M1M3 - Slewing analysis - Positioning

SITCOM - 730 : Create data analysis script/notebook for LVV-T235

SITCOM - 854 : Update LVV-T235 notebook

SITCOM - 810 : Create data analysis script/notebook for LVV-T235 - Raise/Park Repeatability


SITCOM - 797 : M1M3 - Slewing analysis - Positioning
====================================================

Analysing the positions and rotations of xzy compared between before and after the slew. Analysis done with the different elevation angles slewed. 

.. image:: images/797_position_rotation_xyz.png
  :width: 700px

Figure 1. Position moved on xyz between before and after the slew, according to the elevation slew angle

The requirement specifies that the rms repeatability of the mirror positioning is what we need. Plot the rms for each of the distributions shown in the violin plot

.. image:: images/797_histogram_position_rotation_xyz.png
  :width: 700px

Figure 2. Histogram for position and rotation xyz for after - before 


SITCOM - 854 : Update LVV-T235 notebook
========================================

Assume that the systematic offset seen in each of the Rotation panels is a calibration issue, and remove the median value from each panel to assess the scatter about the median.

.. image:: images/854_rotation_sub_median.png
  :width: 700px

Figure 3. Position errors and rotation - median 

The requirement specifies that the rms repeatability of the mirror positioning is what we need. Plot the rms for each of the distributions shown in the violin plot:

.. image:: images/854_rms_repeatability.png
  :width: 700px

Figure 4. rms repeatability

SITCOM - 810 : Create data analysis script/notebook for LVV-T235 - Raise/Park Repeatability
============================================================================================
Notebook to fit a line to the final raised position and confirm that the slope is close to 0. The script for the test case can be found here: `http://lsst-ts/ts_m1m3supporttesting/M13T012.py`

.. image:: images/810_slope_plot.png
  :width: 700px

Figure 5. x position between successive ACTIVEENGINEERING and LOWERINGENGINEERING states, and between selected timestamps where all 6 Hard Points are in standby `(motionState == 0)`. 

.. image:: images/810_table_slopes.png
  :width: 700px
 
Figure 6. Statistics on slopes in `mm/s`


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
