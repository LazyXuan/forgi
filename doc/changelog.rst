Changelog
=========

Changes in Version 0.4
----------------------

*  In `forgi.graph.bulge_graph`:

   *  Speed improvement: Basepair distances between elements are cached.
   *  The Bulge-graph object and file format supports arbitrary key-value pairs in the `info` dictionary.
   *  `BulgeGraph.get_connected_nucleotides` no longer sorts the 
      output nucleotides. Now this function depends on the order of stem1 and stem2 and can thus be used 
      to determine the direction of a bulge. This is used in the new member function `get_link_direction`.
   *  Added function `BulgeGraph.get_domains`, to return a lists of pseudoknots, multiloops and rods. 
      The interface of this function might change in the future.
   *  Merged pull-request by tcarlile for `forgi.graph.bulge_graph`:

      *  `BulgeGraph.get_stem_edge(self, stem, pos)`: Returns 0 if pos on 5' edge of stem, returns 1 if pos on 3' edge of stem.
      *  `BulgeGraph.shortest_path(self, e1, e2)`:    Returns a list of the nodes along the shortest path between e1, and e2.

*  Restructured forgi.threedee.model.comparison and forgi.threedee.utilities.rmsd into
   `forgi.threedee.model.similarity` and `forgi.threedee.model.descriptors`
   The `similarity` module contains all functions for the comparison of two point 
   clouds or two cg structures.
   The `descriptors` module contains functions for describing a single point cloud, 
   such as the radius of gyration or new functions for the gyration tensor.
*  `average_stem_vres_positions` are back with recalculated values
*  Changes in `forgi.threedee.model.coarse_grain` to the `CoarseGrainRNA` object:

   *  In the `self.coords` dictionary, the start and end coordinate are now in a consistent order.
   *  Call new member function `self.add_all_virtual_residues` instead of `forgi.threedee.utilities.graph_pdb.add_virtual_residues`
   *  Coordinates of interior loops and multiloop segmentsa are no longer stored in the cg-files, 
      as they can be deduced from stem coordinates.

      * New member function `self.add_bulge_coords_from_stems` is provided instead 
        of function `forgi.threedee.utilities.graph_pdb.add_bulge_information_from_pdb_chain`

   *  Member function `self.get_virtual_residue(pos)` is provided for easier access than directly via `self.v3dposs`.
      For single stranded regions, virtual residue positions along the direct line of the coarse 
      grain element can be estimated optionally.
      Virtual residues are cached and the cache is automatically cleared when 
      the coordinates or twists of the coarse grained RNA are changed.
   * Functions for creating coordinate arrays for the structure

      * `self.get_ordered_stem_poss` for the start and end coordinates of stems.
      * `self.get_ordered_virtual_residue_poss` for all virtual residue coordinates of stems.
        Replaces `forgi.threedee.utilities.graph_pdb.bg_virtual_residues`
      * `self.get_poss_for_domain` for coordinates only for certain coarse grained elements.

   *  Removed the addition of a pseudo-vector to loop stats in `get_loop_stats`, which was used to avoid zero-length bulges.
      Now 0-length bulges are possible. This makes saving and loading stats consistent.
   *  `self.get_coordinates_array` now returns a 2D `nx3 numpy array` holding n coordinate entries.
      You can use numpy's `.flatten()` to generate a 1D array. If you want to load a 1D flat coordinate array `a`, use
      `self.load_coordinates_array(a.reshape((-1,3))`
   *  Overrides the newly introduced method `sorted_edges_for_mst` from BulgeGraph. 
      Now elements that have no `sampled` entry are broken preferedly.
      This should ensure that the minimal spanning tree is the same after saving and loading an 
      RNA generated with the program Ernwin to/from a file.
   *  `self.coords_to_directions` and `coords_from_directions`:
      Export the coordinates as an array of directions (end-start). 
      This array has only 1 entry per coarse grained element.

*  In `forgi.threedee.model.stats`: Added class for clustered angle stats.
*  Changes in `forgi.projection.hausdorff`.
*  Changes in `forgi.projection.projection2d`

   *  Faster rotation and rasterization.
   *  selected virtual residues can be included in the projection

*  In `forgi.threedee.utilities.graph_pdb`: Added functions `get_basepair_center` and `get_basepair_plane`.
   This will be used in the future for stacking detection.

Changes in Version 0.3
----------------------

*  CoarseGrainRNA now has a member `cg.virtual_atoms()` which is used for caching of virtual atom positions.
   `forgi.threedee.utilities.graph_pdb.virtual_atoms()` now only calculates atom positions when they are needed.
   The two changes together lead to a great speed improvement.
*  The subpackage `forgi.visual` was started for easy visualization of RNA using fornac or matplotlib.
   This subpackage is in an early development stage and will be improved in future versions.
*  The subpackage forgi.projection was added to work with projections of CoarseGrainedRNA objects onto a 2D plane.
*  Now `forgi.threedee.model.average_atom_positions` is used for all virtual atom calculations 
   while `average_stem_vres_positions` has been removed. This leads to more consistent virtual atom calculations.
   Further more the values in `average_atom_positions` have been recalculated.
*  In `forgi.threedee.model.rmsd`, the functions `centered_rmsd` and `centered_drmsd` have been 
   deprecated and deleted respectively. Use `rmsd(cg1,cg2)` for a centered RMSD. This removes code duplication.
*  In `forgi.threedee.model.comparison` a ConfusionMatrix class was introduced for speedup with 
   repeated comparisons to the same reference.
*  Several smaller changes and improvements
