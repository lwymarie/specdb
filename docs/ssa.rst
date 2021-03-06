.. highlight:: rest

******************
IVOA SSA Interface
******************

Although the databases of *specdb* are primarily intended
to be accessed on one's local machines, the repository
includes software to add additional meta-data for IVOA
compliance and to generate VOTables from a *specdb*
database for standard Simple Spectral Access (SSA)
queries.

For the following, we have referred to the Simple
Spectral Access Protocal, Version 1.1 document
and the IVOA working draft 20110319
of Version 2.0 of the Spectral Data Model (dated 20160928).

Notebooks
=========

.. toctree::
   :maxdepth: 1

       SSA <SSA_IVOA>

Meta Data
=========

See the :doc:`private` documentation for tips on how
to add meta data for IVOA compliance.

SSA Interface
=============

The repository includes a simple class `SSAInterface`
which ingests a *specdb* database and enables standard
SSA queries.

Instantiation
-------------

Instantiation is straightforward::

   from specdb import ssa as spdb_ssa
   ssai = spdb_ssa.SSAInterface(igmsp)

querydata
---------

One may perform a standard SSQ querydata using the interface.
Currently, only the POS, SIZE, and FORMAT parameters are
enabled::

   # votable = ssai.querydata(POS, SIZE=, FORMAT=)
   votable = ssai.querydata('0.0019,17.7737', SIZE=1e-3)

The method returns a VOTable generated by astropy.
See below for a listing of the standard meta parameters.

METADATA
--------

As per SSA protocol, a data query with FORMAT=METADATA::

   votable = ssai.querydata(FORMAT='METADATA')

will return the default input and output parameters of the service.
The following shows the current implementation.

Referring to the Version 2.0 of the Spectral Data Model, all
of the mandatory parameters are included except:

======================================= =====================================================
Field                                   Reason
======================================= =====================================================
Char.SpatialAxis.Coverage.Bounds.Extent The aperture is not always precisely defined or known
Char.TimeAxis.Coverage.Bounds.Extent    The total exposure time has not always been recorded and
 ..                                     spectra are often the combination of ones acquired over mutiple nights
======================================= =====================================================

Also, Target.Name is simply the data group name with the GROUP_ID appended.


`specdb SSA METADATA <https://github.com/specdb/specdb/blob/usage/docs/ssa_metadata.xml>`_
shows the current METADATA response.
