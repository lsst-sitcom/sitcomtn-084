###############################
Position Repeatability Analysis
###############################

.. abstract::

   This is the technote for the Position Repeatability Analysis on the TMA with M1M3 



.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

Abstract
========

This is the technote for the Position Repeatability Analysis on the TMA with M1M3. We measured the mirror motion (position and rotation) before and after a slew. 
Analysis was done for slews reaching different elevation and azimuth angles, and also for different slewing speed.  

Related SITCOM tickets
======================

SITCOM - 797 : M1M3 - Slewing analysis - Positioning

SITCOM - 730 : Create data analysis script/notebook for LVV-T235

SITCOM - 854 : Update LVV-T235 notebook

SITCOM - 810 : Create data analysis script/notebook for LVV-T235 - Raise/Park Repeatability

SITCOM - 1118 : M1M3 slewing analysis when TMA moves in azimuth only


SITCOM - 797 : M1M3 - Slewing analysis - Positioning
====================================================
Related notebook: 
`SITCOM-797_ims_slewing_analysis_positioning <https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m1m3/SITCOM-797_ims_slewing_analysis_positioning.ipynb>`__.

For this analysis, we compare the positions and rotations reported by the IMS before and after a slew. The analysis is done using different elevation and azimuth angles and speed. The measurement of the mirror positions / rotations before the slew is performed by averaging the positions / rotations reported by the IMS over a 2 second time window right before the slew. The measurement of the mirror positions / rotations after the slew is performed by averaging the positions / rotations reported by the IMS over a 2 second time window computed after a 6 second settle time after the end of the slew.

In order to benefit from the latest tuning of the M1M3 + TMA system, we have selected data acquired during the last 3 days of operation before the january 2024 shutdown. Slews are selected using the TMAEventMaker utility and slews with no blockInfos are rejected. This allows to avoid spurious data.

The initial requirements are the following:

- the mirror decentering (x and y) is less than +/- 6 micrometers
- the mirror tilt is less than +/- 24e-6 degrees (0.0864 arcsec)
- piston (z displacement) is less than +/- 1 micrometer

.. figure:: _static/TN084-elev-histo.png
  :width: 700px

Figure 1. Histogram of position and rotation difference after - before slew. The red lines indicate the tolerance intervals 

.. figure:: _static/TN084-elev-scatter-1.png
  :width: 700px

Figure 2. x, y, z position and rotation shifts as a function of the elevation angle difference. The color scale shows the elevation speed and the red lines indicate the tolerance intervals. 

.. figure:: _static/TN084-elev-scatter-2.png
  :width: 700px

Figure 3. Correlations between the position / rotation shifts in x, y, z. The color scale shows the elevation angle difference during the slew and the red lines indicate the tolerance intervals.

.. image:: _static/TN084-elev-slew.png
  :width: 700px

Figure 4. Mirror positions and rotations as a function of time as reported by the IMS during a large (70.5 degrees) elevation slew.  The vertical red lines show the start of the slew, the green ones show the end of the slew and the rightmost black ones correspond to the end of the slew plus the 6s settle time.  

From the Figure 4 we can see large deviations in the mirror positions and rotations probably associated to the acceleration and deceleration phases ot the TMA movement. We also see that allowing a 6s settle time is necessary to avoid some large fluactuations after the end of the slews and to get a reasonable estimation of the final mirror positions and rotations.

From these plots we conclude that the initial specifications on the mirror positions and rotations are not met, especially for the piston (z displacement). After discussion with experts, it was realized that these displacements are normal and correspond to the sag of the mirror cell due to gravity change that should be compensated by adjusting the M2 and camera hexapods. 

SITCOM - 1118 : Slewing in Azimuth Analysis - Positioning
=========================================================
Related notebook:
`SITCOMN-084--ims_slewing_azimuth_analysis_positioning <https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m1m3/SITCOMTN-084-ims_slewing_azimuth_analysis_positioning.ipynb>`__.

This analysis is very similar to the one described in the previous section but this time we select slews where the TMA is moving in azimuth only. In this case we do not expect significant forces to be applied to the M1M3 mirror cell. In the same way as before we allow a 6s settle time before measuring the final mirror positions and rotations.

.. figure:: _static/TN084-azi-histo.png
  :width: 700px

Figure 5. Histograms of the x, y, z position and rotation displacements. The red lines shows the targeted tolerances

.. figure:: _static/TN084-elev-scatter-1.png
  :width: 700px

Figure 6. x, y, z position and rotation displacements as a function of the azimuth angle difference. The color scale shows the speed in azimuth and the red lines indicate the tolerance intervals.

In this analysis we see that the mirror displacements are within the specifications but for the rotation around the x axis where there are some outliers. The mirror rotation seems also correlated to the azimuth difference for movement < 50 degrees. For larger TMA movements the mirror rotation is within the specifications. This behavior needs to be confirmed on a larger statistics.

SITCOM - 797 : M1M3 - Compensating mirror motion with the hexapods.
===================================================================
Related notebook: `SITCOM-797_MTM1M3_Position_Stability_17Nov23 <https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m1m3/SITCOM-797_MTM1M3_Position_Stability_17Nov23.ipynb>`__.

The strategy to compensate the mirror cell displacement by adjusting the M2 and camera hexapods requires that the mirror motion be reproducible, so that a look-up table for the hexapods will be able to follow the motion to the specified tolerances.  So the next step was to look at a large number of slews taken during a random walk session and look at the mirror position as a function of elevation for each of the slews.  This is shown in Figure 4.  The colors are the slew speed as a percentage of maximum speed. The curves seem to group themselves according to the slew speeds.  However, note the speed grouping is ordered as 30-40-50-20, which is the order that the tests were run in.  So it is more likely that the grouping reflects something else varying during the night.  One possibility is that the variation is caused by temperature.  Figure 5 shows the Z-position variation through the same group of slews. Even though the temperature change during the run was only 0.6C, compensating for temperature significantly reduces the scatter in the points. However, note that we are still a factor of 3-5 away from meeting the specification.  More testing is needed to see if we can reduce the scatter to the required level.


.. figure:: _static/Final_Mirror_Position_AzLimits_3_03Aug23.png
  :width: 700px

Figure 7. Mirror position as reported by the IMS at the end of many slews during a random walk session. Detailed explanation in the text.    


.. figure:: _static/Mirror_Position_Temperature_03Aug23.png
  :width: 700px

Figure 8. Mirror Z-position as reported by the IMS at the end of many slews during a random walk session. The temperature during the night is shown in the lower left.  Compensating for temperature significantly reduces the scatter in the points, but we are still a factor of 3-5 away from meeting the specification.

SITCOM - 797 - Position and rotation repeatibility for a specific target elevation.
===================================================================================
Related notebook: 
`SITCOM-797_ims_slewing_analysis_positioning <https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m1m3/SITCOM-797_ims_slewing_analysis_positioning.ipynb>`__.

In this analysis we select large amplitude slews (elevation difference > 10 degrees) where the target elevation is 45 +/- 2 degrees and we check the posision / rotation shift with respect to the average position / rotation for this specific elevation angle. Unfortunately we find only 19 slews with such characteristics but it seems that the position / rotation shifts are closer to the specifications with respect to the case where the target elevation is not fixed to a specific value. This study would have to be repeated with a larger statistics to draw firm conclusions.

.. figure:: _static/TN084-elev-fix-45-histo.png

Figure 9. Mirror position / rotation shifts for target elevation equal to 45 +/- degrees. Detailed explanation in the text.  

SITCOM - 1159 - Detailed analysis of a slew where the TMA is moving in azimuth only
===================================================================================

.. figure:: _static/TN084-azi-slew-1.png

Figure 10. Variation as a funtion of time of the inclinometer and azimuth measurement (left), the rotation around the x axis (middle) and the total applied balance force along the x axis (right) for a single slew. 

The azimuth measurement is flat all along the slew but we know that a time delay makes this measurement unreliable. The inclinometer measurement is supposed to give the angle of rotation of the mirror around the x axis but it is also sensitive to the acceleration of the TMA. This effect is clearly seen in the left panel of the previous figure. As the inclinometer data is part of the mirror position control loop, the system is probably over-compensating the mirror rotation around the x axis, leading to mirror displacements out of specification. A possible way to overcome this problem would be to fit the inclinometer values to the measured applied forces in order to be able to subtract the effect of the acceleration from the inclinometer data. This will be attempted in a separate analysis.

SITCOM - 854 : Update LVV-T235 notebook
========================================

Assume that the systematic offset seen in each of the Rotation panels is a calibration issue, and remove the median value from each panel to assess the scatter about the median.

.. figure:: _static/854_rotation_sub_median.png
  :width: 700px

Figure 11. Position errors and rotation - median 

The requirement specifies that the rms repeatability of the mirror positioning is what we need. Plot the rms for each of the distributions shown in the violin plot:

.. figure:: _static/854_rms_repeatability.png
  :width: 700px

Figure 12. rms repeatability

SITCOM - 810 : Create data analysis script/notebook for LVV-T235 - Raise/Park Repeatability
============================================================================================

Related notebook: `LVV-T235-SITCOM-810 <https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m1m3/LVV-T235-SITCOM-810.ipynb>`__.

In this analysis we use the data from the LVV-T235 test case where the mirror cell is repeatedly lowered and raised. We fit a line to the final raised position as measured by the IMS in order to verify that the slope is compatible with 0 mening that the mirror cell has reached a stable position and is not moving anymore.

The following plot shows the x and y positions (shifted in order to be centered around 0) as a function of time, the vertical lines indicate the various M1M3 "detailedStates". We see that the same pattern is repeated several times over the chosen data taking period.

.. figure:: _static/810_overview_ref_subtracted.png
  :width: 700px

Figure 13. x and y positions while lowering and raising the mirror

If we isolate one of the patterns we get the following figure where the lines show the Hard Point states

.. figure:: _static/810_singleloop.png
  :width: 700px

Figure 14. Zoom on a single lowering / raising cycle

We are interested in the slope of the x and y positions on the flat tops / bottoms just before reaching the state: "HP in Standby" (indicated by the dotted lines). We choose to select and analyze one second of data before "HP in standby". During this last second the position is expected to be stable, showing a slope which is compatible with 0.

We repeat this selection for every cycle in x and y and get a set of plots like the following showing the x and y positions as a function of time in the one-second time windows.

.. figure:: _static/810_position_stability.png
  :width: 700px

Figure 15. x and y positions during the last second before reaching the "satnadby" state for typical cycles

The slope measurement is performed on every subset of data. Statistics are summarized in the following table

.. figure:: _static/810_table_slopes.png
  :width: 700px

We conclude that on this dataset, the slopes of x and y positions as a function of time are all compatible with 0 

Reference links
=================
<http://lsst-ts/ts_m1m3supporttesting/M13T012.py>

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

TMA (Telescope Mount Assembly)