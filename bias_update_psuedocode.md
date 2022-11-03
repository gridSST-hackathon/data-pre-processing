**Pseudocode for bias correction using CMC as reference**

This requires the creation and daily updating of bias correction fields for each input datatype (e.g. VIIRS daytime or MetOp nighttime).  The bias correction files can be stored on the same 0.1x0.1 degree grid as the CMC and values for each L2P pixel (i.e. in satellite projection) obtained by bilinear interpolation.  Bias corrections should be low-pass (~2x2 degrees) and not simply consist of obs - reference values for every pixel, which will just result in obtaining the reference field if the correction is applied.  The final bias for each L2P pixel can be obtained using bilinear interpolation of the new 0.1x0.1 degree bias field.

Andy Harris, 11/03/2022

Example parameters in external config file:

bias_relax=0.9
bias_weight=[0.6,0.4]
n_smooth_x=n_smooth_y=21

% Read in old_bias (i.e. for previous day), today's bias reference (i.e. CMC), today's input observations

obs_bias(nx_b, ny_b)
bias_old(nx_b, ny_b)
bias_new(nx_b, ny_b)
sum_obs(nx_b, ny_b)
n_obs(nx_b, ny_b)

smooth_kernel(n_smooth_x, n_smooth_y)

%  For example, a simple smooth, but could use a different (e.g. Gaussian) kernel

smooth_kernel(:,:)=1

nx_b, ny_b = dimensions of x,y in bias field (i.e. on 0.1x0.1 degree grid)
nx, ny = dimensions of x,y  in input_datatype (i.e. L2P pixels)

obs=input_datatype(good)

good is a vector of indices for input data that pass QC
n_good is the length of the vector

n_obs(:,:)=0
sum_obs(:,:)=0

loop j=0:n_good-1
    [i1,i2]=get_indices(obs[j].lat,obs[j].lon)
    sum_obs(i1,i2)+=obs[j].sst
    n_obs(i1,i2)+=1
    
ii=where(n_obs>0)

obs_bias(ii)=bias_weight(0)*sum_obs(ii)/n_obs(ii) + bias_weight(1)*bias_old(ii)

jj=where(n_obs==0)

%  Relax back towards zero (e.g. by 10%)
obs_bias(jj)=bias_relax*old_bias(jj)

%  Simple kernel smoothing function.  Can just ignore edges, or perhaps reflect.
bias_new=smooth(obs_bias,smooth_kernel)

%  Save new bias field for given datatype for today's date

save(bias_new, date, datatype)
