using namespace XrdCl;

//Erasure 
void ECWrite(uint64_t  offset,
                       uint32_t  size,
                       const void *buffer,
                       ResponseHandler *userHandler)
{
    std::vector<Pipeline> writes;
    writes.reserve(n_chunks);
    for(size_t id=0;id<n_chunks;++id)
    {
        //compute offset, size and buffer for each stripe/chunk
        File *file=new File();
        Pipeline p=Open(file, url, flags)
                        | Parallel(Write(file, chunk_offset, chunk_size,chunk_buffer)),
                                        SetXAttr(file, "xrdec.chksum",checksum))
                       | Close(file) >> [file](XRootDStatus &){delete file};}                
    }
    //execute the workflow asynchronoulsy
    Async(Parallel(writes))>> [userHandler](XRootDStatus & status)
                                                { userHandler->HandleResponse(new XRootDStatus(status),0);});
}
