###Project description:
   The goal is to simulate wall roughness in a channel flow. The roughness is approximated with a pattern of truncated cones. The height of the roughness elements is below 20 wall units, which introduces many elements in order to resolve the elements. In the bulk flow less elements could be used. However currently the distorted mesh at the wall is extruded into the bulk region, which leads to high aspect ratio elements and therefore to an unfavorable Courant number distribution. The idea is to use NekNek which couples the distorted mesh (wall region) with a high quality genbox mesh in the bulk flow featuring "correct" sized elements.
   
   
###The issue:   
   Several tests for the smooth wall case using the roughness mesh initialzed with "ref.fld" field have shown the following:
   - without periodic boundary conditions - using inflow outflow in streamwise and wall in spanwise direction - everything works fine
   - with streamwise periodic boundary conditions the flow field explodes right after few timesteps (in timestep 2 GMRES takes 500 iterations), see "neknek/logfile_neknek" and "pictures/velocity_neknek_3d.png"
   - just as a sanity check, with singlesession Nek everything works fine, see "singlesession/logfile_mfull" and "pictures/velocity_singlesession_3d.png"
   - changing the following parameters do not seem to solve the issue:
      - time scheme order from 2 to 3
      - ngeom/ninter
      - pressure tolerance
      - filtering
      - residualProj for pressure
      - targetCFL
      - using genbox mesh for the wall region
      - using Nek5000 v19
      
###How I ran the test case (I am using Nek5000 v17):
   1. export PPLIST="NEKNEK"
   2. makenek sim (compiler flags are mcmodel=medium for Fortran and C)
   3. cp nek5000 neknek/
   4. cp ref.fld neknek/
   5. cd neknek
   6. neknek mwall mbulk <nwall> <nbulk> (In the test I used 15 for each session)
   
