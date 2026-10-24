# 3D Gaussian Ray Tracer

An OptiX-based differentiable 3D Gaussian Ray Tracer, inspired by the work [3D Gaussian Ray Tracing: Fast Tracing of Particle Scenes](https://gaussiantracer.github.io/). See [gaussian-raytracing](https://github.com/LogosIntelligenceLab/gaussian-raytracing) for 3D reconstruction.

### Install
```bash
# clone the repo
git clone https://github.com/LogosIntelligenceLab/gtracer.git
cd gtracer

# use cmake to build the project for ptx file (for Optix)
rm -rf ./build && mkdir build && cd build && cmake .. && make && cd ../

# Install the package
pip install .
```

### Example usage
Python API:
```python
from gtracer import _C
# create a Gaussian Ray Tracer
bvh = _C.create_gaussiantracer()

# build it with triangles associated with each Gaussian
bvh.build_bvh(vertices_b[faces_b])

# update the vertices in bvh if you already build it, faster than build_bvh. 
# But the topology and the number of triangle faces should keep the same.
bvh.update_bvh(vertices_b[faces_b])

# trace forward
bvh.trace_forward(
    rays_o, rays_d, gs_idxs, means3D, opacity, SinvR, shs, 
    colors, depth, alpha, 
    alpha_min, transmittance_min, deg,
)

# trace backward
bvh.trace_backward(
    rays_o, rays_d, gs_idxs, means3D, opacity, SinvR, shs, 
    colors, depth, alpha, 
    grad_means3D, grad_opacity, grad_SinvR, grad_shs,
    grad_out_color, grad_out_depth, grad_out_alpha,
    ctx.alpha_min, ctx.transmittance_min, ctx.deg,
)
```
Example usage:
```bash
cd example
# Interactive viewer for 3DGS format point cloud
python renderer.py -p point_cloud.ply
```
### Acknowledgement

* Credits to [Instant-NGP](https://github.com/NVlabs/instant-ngp) and [raytracing](https://github.com/NVlabs/instant-ngp).
* Credits to the original [3D Gaussian Ray Tracing](https://gaussiantracer.github.io/) paper.


## 📜 Citation
If you find this work useful for your research, please cite our github repo:
```bibtex
@misc{gu2024gtracer,
    title = {OptiX-based differentiable 3D Gaussian Ray Tracer},
    author = {Gu, Chun and Zhang, Li},
    howpublished = {\url{https://github.com/fudan-zvg/gtracer}},
    year = {2024}
}
```
