<template>
    <div>
        <div class="w-full flex justify-end gap-3 mb-4 -mt-10 ">
            <button
                class="flex py-2 px-3 text-sky-500  gap-2 border-2 bg-white rounded-md text-center  hover:border-sky-500 duration-300">
                <img src="../../assets/uploade.svg" alt="">
                <p>นำเข้าข้อมูล</p>
            </button>

            <button @click="showModal = true"
                class="flex py-2 px-3 text-white justify-center items-center  gap-2 bg-sky-500 rounded-md text-center  hover:border-white hover:bg-green-600 duration-300">
                <Icon icon="ic:baseline-plus" width="24" height="24" />
                <p>เพิ่มทะเบียนพิเศษ</p>
            </button>
            <div class="min-w-[304px]"></div>
            <n-modal id="post-registration" v-model:show="showModal" preset="card"
                class=" w-full h-screen !text-white !bg-black p-4 noto-serif-thai">
                <template #header>
                    <div class="text-white">จัดการทะเบียนพิเศษ</div>
                </template>

                <main class="border-sky-500 border h-full p-6 pr-0 flex  min-w-full ">

                    <div id="add-special-registration" class="border-r p-4 w-full relative">
                        <div>
                            <section id="first-line" class="flex gap-12 w-full whitespace-nowrap">
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">หมวดอักษร*</label>
                                    <n-input v-model:value="addSpecialRegistration.letter" type="text"
                                        placeholder="กรุณากรอกอักษร" />
                                </div>
                                <span class="w-full flex flex-col gap-y-2">
                                    <label for="">ป้ายทะเบียน*</label>
                                    <n-input v-model:value="addSpecialRegistration.numberId" type="text"
                                        placeholder="กรุณากรอกป้ายทะเบียน" />
                                </span>
                                <span class="w-full flex flex-col gap-y-2">
                                    <label for="">จังหวัด*</label>
                                    <n-select v-model:value="addSpecialRegistration.province" :options="provinces" />
                                </span>
                            </section>

                            <section id="seconde-line" class="w-full flex gap-12  my-8">
                                <span class="w-full flex flex-col gap-y-2">
                                    <label for="">กลุ่มทะเบียน*</label>
                                    <n-select v-model:value="addSpecialRegistration.groupSpecialRegistration"
                                        :options="groupSpecialRegistration" placeholder="กรุณาเลือกกลุ่มทะเบียน"
                                        class="my-2" />
                                </span>
                                <div class="w-full h-1"></div>
                                <div class="w-full h-1"></div>
                            </section>

                            <section id="third-line" class="flex gap-12 w-full whitespace-nowrap my-8">
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">หมายเลขคดี</label>
                                    <n-input v-model:value="addSpecialRegistration.caseId" type="text"
                                        placeholder="กรุณากรอกหมายเลขคดี" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">วันที่ออกหมายจับ</label>
                                    <n-date-picker v-model:value="addSpecialRegistration.addDate" type="date" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">วันที่สิ้นสุดออกหมายจับ</label>
                                    <n-date-picker v-model:value="addSpecialRegistration.fixDate" type="date" />
                                </div>
                            </section>

                            <section id="fourth-line" class="flex flex-col gap-2 w-full whitespace-nowrap my-8 ">
                                <label for="">พฤติการ</label>
                                <n-input v-model:value="addSpecialRegistration.behavior" type="textarea"
                                    placeholder="กรุณาอธิบายพฤติการ" />
                            </section>

                            <section id="five-line" class="flex gap-12 w-full whitespace-nowrap my-8">
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">เจ้าของข้อมูล</label>
                                    <n-input v-model:value="addSpecialRegistration.ownUser" type="text"
                                        placeholder="กรุณากรอกชื่อเจ้าของข้อมูล" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">หน่วยงาน</label>
                                    <n-input v-model:value="addSpecialRegistration.agency" type="text"
                                        placeholder="กรุณากรอกหน่วยงาน" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">เบอร์ติดต่อ</label>
                                    <n-input v-model:value="addSpecialRegistration.phoneNumber" type="text"
                                        placeholder="กรุณากรอกหมายเลขโทรศัพท์" />
                                </div>
                            </section>

                            <n-checkbox size="large" label="Active" v-model:checked="addSpecialRegistration.status" />
                        </div>

                        <div class="flex absolute bottom-0 right-4 justify-end items-center gap-2 mt-auto pt-4 ">
                            <button @click="submitCallback"
                                class="flex py-2 px-3 text-white gap-2 bg-sky-500 border-sky-500 border rounded-md text-center hover:bg-green-500 duration-300">
                                <Icon icon="material-symbols:save-outline" width="20" height="20" />
                                <p class="whitespace-nowrap">บันทึก</p>
                            </button>
                            <button @click="cancelCallback"
                                class="flex py-2 px-6 text-sky-500 gap-2 border border-sky-500 bg-white rounded-md text-center hover:bg-red-600 hover:text-white hover:border-white duration-300">
                                <p>ยกเลิก</p>
                            </button>
                        </div>
                        <n-modal v-model:show="submitFailed" preset="card"
                            class="absolute top-0 left-0 bottom-0 right-0 bg-black w-[500px] h-fit pb-20 z-50 rounded-lg noto-serif-thai">
                            <div class="flex flex-col justify-center items-center gapy-4">
                                <Icon icon="heroicons:x-circle-16-solid" width="100" style="color: #ff0000" />
                                <h1 class="my-4 text-3xl font-bold">บันทึกไม่สำเร็จ</h1>
                                <p class="text-lg ">กรุณากรอกข้อมูลหรืออัพโหลดไฟล์ให้ครบถ้วน</p>
                            </div>

                        </n-modal>

                    </div>

                    <div class="flex flex-col  p-4 max-w-">
                        <div @click="triggerFileInput"
                            class="bg-zinc-600 max-w-[350px] h-64 flex flex-col items-center justify-center mb-4 cursor-pointer">
                            <img v-if="uploadedImage" :src="uploadedImage" alt="Uploaded Image"
                                class="max-w-full max-h-full" />
                            <span v-else>
                                <img src="../../assets/uploade-photo.svg" alt="Upload Photo">
                                <p class="text-zinc-400 text-center">อัพโหลดรูปภาพ</p>
                            </span>

                        </div>

                        <div class="flex gap-1  justify-center items-center">
                            <label
                                class="p-2 whitespace-nowrap bg-white text-gray-400 rounded-md w-[180px] py-[9px] cursor-pointer">
                                <p>แนบไฟล์</p>
                                <input type="file" class="hidden" ref="fileInputRef" @change="handleFileUpload" />
                            </label>

                            <button type="file"
                                class="flex py-2 px-3 text-white gap-2 bg-sky-500 rounded-md text-center duration-300"
                                @click="triggerFileInput">
                                <svg width="24" height="24" viewBox="0 0 24 24" fill="white"
                                    xmlns="http://www.w3.org/2000/svg">
                                    <path
                                        d="M11 9.85L10.125 10.725C9.925 10.925 9.6875 11.0208 9.4125 11.0125C9.1375 11.0042 8.9 10.9 8.7 10.7C8.51667 10.5 8.42083 10.2667 8.4125 10C8.40417 9.73333 8.5 9.5 8.7 9.3L11.3 6.7C11.4 6.6 11.5083 6.52917 11.625 6.4875C11.7417 6.44583 11.8667 6.425 12 6.425C12.1333 6.425 12.2583 6.44583 12.375 6.4875C12.4917 6.52917 12.6 6.6 12.7 6.7L15.3 9.3C15.5 9.5 15.5958 9.73333 15.5875 10C15.5792 10.2667 15.4833 10.5 15.3 10.7C15.1 10.9 14.8625 11.0042 14.5875 11.0125C14.3125 11.0208 14.075 10.925 13.875 10.725L13 9.85V13C13 13.2833 12.9042 13.5208 12.7125 13.7125C12.5208 13.9042 12.2833 14 12 14C11.7167 14 11.4792 13.9042 11.2875 13.7125C11.0958 13.5208 11 13.2833 11 13V9.85ZM5 21C4.45 21 3.97917 20.8042 3.5875 20.4125C3.19583 20.0208 3 19.55 3 19V5C3 4.45 3.19583 3.97917 3.5875 3.5875C3.97917 3.19583 4.45 3 5 3H19C19.55 3 20.0208 3.19583 20.4125 3.5875C20.8042 3.97917 21 4.45 21 5V19C21 19.55 20.8042 20.0208 20.4125 20.4125C20.0208 20.8042 19.55 21 19 21H5ZM5 19H19V16H16C15.5 16.6333 14.9042 17.125 14.2125 17.475C13.5208 17.825 12.7833 18 12 18C11.2167 18 10.4792 17.825 9.7875 17.475C9.09583 17.125 8.5 16.6333 8 16H5V19ZM12 16C12.5333 16 13.025 15.8625 13.475 15.5875C13.925 15.3125 14.2917 14.95 14.575 14.5C14.675 14.35 14.8 14.2292 14.95 14.1375C15.1 14.0458 15.2667 14 15.45 14H19V5H5V14H8.55C8.73333 14 8.9 14.0458 9.05 14.1375C9.2 14.2292 9.325 14.35 9.425 14.5C9.70833 14.95 10.075 15.3125 10.525 15.5875C10.975 15.8625 11.4667 16 12 16Z"
                                        fill="white" />
                                </svg>
                                <p class="whitespace-nowrap">Upload</p>
                            </button>

                        </div>

                        <div class=" mt-4  bg-zinc-600 font-normal">
                            <div v-if="uploadedFiles.length === 0" class="border-b p-2">ไม่มีข้อมูล</div>
                            <div v-else>
                                <div v-for="file in uploadedFiles" :key="file.name"
                                    class="flex justify-between border-b border-dotted p-2">
                                    <p class="max-w-[130px] w-[130px] truncate whitespace-nowrap">{{ file.name }}</p>
                                    <p>{{ file.uploadTime }}</p>
                                    <Icon icon="tabler:trash" width="20" height="20" @click="deleteFile(file.name)"
                                        class="cursor-pointer" />
                                </div>
                            </div>
                        </div>
                    </div>

                </main>

            </n-modal>
            <n-modal v-model:show="submitSave" preset="card"
                class="absolute top-0 left-0 bottom-0 right-0 bg-white w-[500px] h-fit pb-20 z-50 rounded-lg noto-serif-thai">
                <div class="flex flex-col justify-center items-center gapy-4">
                    <img src="../../assets/check.svg" alt="">
                    <h1 class="my-4 text-3xl font-bold">บันทึกสำเร็จ</h1>
                    <p class="text-lg ">ข้อมูลถูกบันทึกเรียบร้อย</p>
                </div>
            </n-modal>
            <n-modal id="patch-registration" v-model:show="showPatchModal" preset="card"
                class=" w-full h-screen !text-white !bg-black p-4 noto-serif-thai">
                <template #header>
                    <div class="text-white">แก้ไขทะเบียนพิเศษ</div>
                </template>

                <main class="border-sky-500 border h-full p-6 pr-0 flex  min-w-full ">

                    <div id="add-special-registration" class="border-r p-4 w-full relative">
                        <div>
                            <section id="first-line" class="flex gap-12 w-full whitespace-nowrap">
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">หมวดอักษร*</label>
                                    <n-input v-model:value="patchData.letter" type="text"
                                        placeholder="กรุณากรอกอักษร" />
                                </div>
                                <span class="w-full flex flex-col gap-y-2">
                                    <label for="">ป้ายทะเบียน*</label>
                                    <n-input v-model:value="patchData.numberId" type="text"
                                        placeholder="กรุณากรอกป้ายทะเบียน" />
                                </span>
                                <span class="w-full flex flex-col gap-y-2">
                                    <label for="">จังหวัด*</label>
                                    <n-select v-model:value="patchData.province" :options="provinces" />
                                </span>
                            </section>

                            <section id="seconde-line" class="w-full flex gap-12  my-8">
                                <span class="w-full flex flex-col gap-y-2">
                                    <label for="">กลุ่มทะเบียน*</label>
                                    <n-select v-model:value="patchData.groupSpecialRegistration"
                                        :options="groupSpecialRegistration" placeholder="กรุณาเลือกกลุ่มทะเบียน"
                                        class="my-2" />
                                </span>
                                <div class="w-full h-1"></div>
                                <div class="w-full h-1"></div>
                            </section>

                            <section id="third-line" class="flex gap-12 w-full whitespace-nowrap my-8">
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">หมายเลขคดี</label>
                                    <n-input v-model:value="patchData.caseId" type="text"
                                        placeholder="กรุณากรอกหมายเลขคดี" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">วันที่ออกหมายจับ</label>
                                    <n-date-picker v-model:value="patchData.addDate" type="date" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">วันที่สิ้นสุดออกหมายจับ</label>
                                    <n-date-picker v-model:value="patchData.fixDate" type="date" />
                                </div>
                            </section>

                            <section id="fourth-line" class="flex flex-col gap-2 w-full whitespace-nowrap my-8 ">
                                <label for="">พฤติการ</label>
                                <n-input v-model:value="patchData.behavior" type="textarea"
                                    placeholder="กรุณาอธิบายพฤติการ" />
                            </section>

                            <section id="five-line" class="flex gap-12 w-full whitespace-nowrap my-8">
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">เจ้าของข้อมูล</label>
                                    <n-input v-model:value="patchData.ownUser" type="text"
                                        placeholder="กรุณากรอกชื่อเจ้าของข้อมูล" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">หน่วยงาน</label>
                                    <n-input v-model:value="patchData.agency" type="text"
                                        placeholder="กรุณากรอกหน่วยงาน" />
                                </div>
                                <div class="w-full flex flex-col gap-y-2">
                                    <label for="">เบอร์ติดต่อ</label>
                                    <n-input v-model:value="patchData.phoneNumber" type="text"
                                        placeholder="กรุณากรอกหมายเลขโทรศัพท์" />
                                </div>
                            </section>

                            <n-checkbox size="large" label="Active" v-model:checked="patchData.status" />
                        </div>

                        <div class="flex absolute bottom-0 right-4 justify-end items-center gap-2 mt-auto pt-4 ">
                            <button @click="summitPatch(patchData.id)"
                                class="flex py-2 px-3 text-white gap-2 bg-sky-500 border-sky-500 border rounded-md text-center hover:bg-green-500 duration-300">
                                <Icon icon="material-symbols:save-outline" width="20" height="20" />
                                <p class="whitespace-nowrap">บันทึก</p>
                            </button>
                            <button @click="cancelCallback"
                                class="flex py-2 px-6 text-sky-500 gap-2 border border-sky-500 bg-white rounded-md text-center hover:bg-red-600 hover:text-white hover:border-white duration-300">
                                <p>ยกเลิก</p>
                            </button>
                        </div>
                        <n-modal v-model:show="submitFailed" preset="card"
                            class="absolute top-0 left-0 bottom-0 right-0 bg-black w-[500px] h-fit pb-20 z-50 rounded-lg noto-serif-thai">
                            <div class="flex flex-col justify-center items-center gapy-4">
                                <Icon icon="heroicons:x-circle-16-solid" width="100" style="color: #ff0000" />
                                <h1 class="my-4 text-3xl font-bold">บันทึกไม่สำเร็จ</h1>
                                <p class="text-lg ">กรุณากรอกข้อมูลหรืออัพโหลดไฟล์ให้ครบถ้วน</p>
                            </div>

                        </n-modal>

                    </div>

                    <div class="flex flex-col  p-4 max-w-">
                        <div @click="triggerPatchFileInput"
                            class="bg-zinc-600 max-w-[350px] h-64 flex flex-col items-center justify-center mb-4 cursor-pointer">
                            <img v-if="uploadedPatchImage" :src="lastImage" alt="Uploaded Image"
                                class="max-w-full max-h-full" />
                            <span v-else>
                                <img src="../../assets/uploade-photo.svg" alt="Upload Photo">
                                <p class="text-zinc-400 text-center">อัพโหลดรูปภาพ</p>
                            </span>

                        </div>

                        <div class="flex gap-1  justify-center items-center">
                            <label
                                class="p-2 whitespace-nowrap bg-white text-gray-400 rounded-md w-[180px] py-[9px] cursor-pointer">
                                <p>แนบไฟล์</p>
                                <input type="file" class="hidden" ref="fileInputPatchRef"
                                    @change="handlePatchFileUpload" />
                            </label>

                            <button type="file"
                                class="flex py-2 px-3 text-white gap-2 bg-sky-500 rounded-md text-center duration-300"
                                @click="triggerPatchFileInput">
                                <svg width="24" height="24" viewBox="0 0 24 24" fill="white"
                                    xmlns="http://www.w3.org/2000/svg">
                                    <path
                                        d="M11 9.85L10.125 10.725C9.925 10.925 9.6875 11.0208 9.4125 11.0125C9.1375 11.0042 8.9 10.9 8.7 10.7C8.51667 10.5 8.42083 10.2667 8.4125 10C8.40417 9.73333 8.5 9.5 8.7 9.3L11.3 6.7C11.4 6.6 11.5083 6.52917 11.625 6.4875C11.7417 6.44583 11.8667 6.425 12 6.425C12.1333 6.425 12.2583 6.44583 12.375 6.4875C12.4917 6.52917 12.6 6.6 12.7 6.7L15.3 9.3C15.5 9.5 15.5958 9.73333 15.5875 10C15.5792 10.2667 15.4833 10.5 15.3 10.7C15.1 10.9 14.8625 11.0042 14.5875 11.0125C14.3125 11.0208 14.075 10.925 13.875 10.725L13 9.85V13C13 13.2833 12.9042 13.5208 12.7125 13.7125C12.5208 13.9042 12.2833 14 12 14C11.7167 14 11.4792 13.9042 11.2875 13.7125C11.0958 13.5208 11 13.2833 11 13V9.85ZM5 21C4.45 21 3.97917 20.8042 3.5875 20.4125C3.19583 20.0208 3 19.55 3 19V5C3 4.45 3.19583 3.97917 3.5875 3.5875C3.97917 3.19583 4.45 3 5 3H19C19.55 3 20.0208 3.19583 20.4125 3.5875C20.8042 3.97917 21 4.45 21 5V19C21 19.55 20.8042 20.0208 20.4125 20.4125C20.0208 20.8042 19.55 21 19 21H5ZM5 19H19V16H16C15.5 16.6333 14.9042 17.125 14.2125 17.475C13.5208 17.825 12.7833 18 12 18C11.2167 18 10.4792 17.825 9.7875 17.475C9.09583 17.125 8.5 16.6333 8 16H5V19ZM12 16C12.5333 16 13.025 15.8625 13.475 15.5875C13.925 15.3125 14.2917 14.95 14.575 14.5C14.675 14.35 14.8 14.2292 14.95 14.1375C15.1 14.0458 15.2667 14 15.45 14H19V5H5V14H8.55C8.73333 14 8.9 14.0458 9.05 14.1375C9.2 14.2292 9.325 14.35 9.425 14.5C9.70833 14.95 10.075 15.3125 10.525 15.5875C10.975 15.8625 11.4667 16 12 16Z"
                                        fill="white" />
                                </svg>
                                <p class="whitespace-nowrap">Upload</p>
                            </button>

                        </div>

                        <div class=" mt-4  bg-zinc-600 font-normal">
                            <div v-if="patchData.images.length === 0" class="border-b p-2">ไม่มีข้อมูล</div>
                            <div v-else>
                                <div v-for="image in patchData.images" :key="image.url"
                                    class="flex justify-between border-b border-dotted p-2">
                                    <p class="max-w-[130px] w-[130px] truncate whitespace-nowrap">{{
                                        formattedImageUrl(image.url) }}
                                    </p>
                                    <p>{{ patchData.uploadTime }}</p>
                                    <Icon icon="tabler:trash" width="20" height="20" @click="deletePatchFile(image.url)"
                                        class="cursor-pointer" />
                                </div>
                                <div v-for="file in uploadedPatchFiles" :key="file.name"
                                    class="flex justify-between border-b border-dotted p-2">
                                    <p class="max-w-[130px] w-[130px] truncate whitespace-nowrap">{{ file.name }}</p>
                                    <p>{{ file.uploadTime }}</p>
                                    <Icon icon="tabler:trash" width="20" height="20" @click="deletePatchFile(file.name)"
                                        class="cursor-pointer" />
                                </div>
                            </div>
                        </div>
                    </div>

                </main>

            </n-modal>
        </div>
        <main class="flex">
            <table class="w-full h-max bg-zinc-800 mr-4">
                <thead class="text-center text-white">
                    <tr>
                        <td class="py-8 px-4" v-for="(header, index) in localData.columns" :key="index">{{ header.title
                            }}
                        </td>
                    </tr>
                </thead>
                <tbody class="text-white">
                    <tr v-for="(row, rowIndex) in localData.row" :key="rowIndex">
                        <td :class="[
                            'border-b border-dotted',
                            colIndex % 2 === 0 ? 'bg-neutral-700' : 'bg-neutral-600',
                            header.key === 'image' ? 'px-1' : 'px-4',
                        ]" v-for="(header, colIndex) in localData.columns" :key="colIndex">
                            <template v-if="header.key === 'action'">
                                <div class="flex gap-4 justify-center">
                                    <Icon class="cursor-pointer" @click="fetchRegistrationsById(row.id)"
                                        icon="mdi:pencil-outline" width="20" height="24" />
                                    <Icon class="cursor-pointer" icon="tabler:trash" width="20" height="24"
                                        @click="deleteRow(rowIndex, row.id)" />
                                </div>
                            </template>
                            <template v-else-if="header.key === 'images'">
                                <div
                                    class="flex gap-1 py-1 justify-center items-center m-auto max-w-[200px] overflow-x-auto min-w-[200px]">
                                    <img class="h-14 object-cover w-full" v-for="(image, index) in row[header.key]"
                                        :src="image.url" :alt="image.key" :key="index" />
                                </div>
                            </template>

                            <template v-else>
                                <div v-if="header.key === 'status'" class="flex items-center justify-center ">
                                    <button v-if="row[header.key] === true"
                                        class="text-center rounded-md p-1 font-bold duration-300 min-w-[80px] bg-green-500 hover:bg-green-600">
                                        <p>Active</p>
                                    </button>
                                    <button v-else
                                        class="text-center rounded-md p-1 font-bold duration-300 min-w-[80px] bg-red-500 hover:bg-red-600">
                                        <p>Inactive</p>
                                    </button>
                                </div>
                                <p v-else class="whitespace-nowrap truncate max-w-[200px]">
                                    {{ row[header.key] }}
                                </p>
                            </template>
                        </td>
                    </tr>
                </tbody>
            </table>
            <div class="-mt-[60px]">
                <div style="clip-path: polygon(0% 0%, 70% 0, 80% 0%, 88% 100%, 0% 100%);"
                    class="bg-blue-500 h-10 w-[200px] relative">
                    <div style="clip-path: polygon(0% 0%, 70% 0, 80% 0%, 88% 100%, 0% 100%);"
                        class="absolute bg-black h-[39px] top-[1px] left-[1px] w-[197px] p-2 z-10 text-white text-center">
                        <div class="flex gap-2">
                            <img src="../../assets/car-search.svg" alt="">
                            <p class="text-white">เงื่อนไขการค้นหา</p>
                        </div>
                    </div>
                </div>

                <div class="border border-blue-500 h-[750px] w-[300px] rounded-tr-md -mt-[1px] text-white p-4 ">
                    <label for="">หมวดอักษร</label>
                    <n-input v-model:value="searchKeyword.letter" type="text" placeholder="กรุณากรอกอักษร"
                        class="my-2" />

                    <label for="">เลขทะเบียน</label>
                    <n-input v-model:value="searchKeyword.numberId" type="text" placeholder="กรุณากรอกเลขทะเบียน"
                        class="my-2" />

                    <label for="">จังหวัด</label>
                    <n-select v-model:value="searchKeyword.province" :options="provinces" class="my-2"
                        placeholder="กรุณาเลือกจังหวัด" />

                    <label for="">ประเภททะเบียน</label>
                    <n-select v-model:value="searchKeyword.groupSpecialRegistration" :options="groupSpecialRegistration"
                        placeholder="กรุณาเลือกประเภททะเบียน" class="my-2" />

                    <label for="">หน่วยงานเจ้าของข้อมูล</label>
                    <n-input v-model:value="searchKeyword.agency" type="text" placeholder="หน่วยงานเจ้าของข้อมูล"
                        class="my-2" />

                    <label for="">สถานะข้อมูล</label>
                    <n-select v-model:value="searchKeyword.status" :options="defaultStatus" class="my-2" />

                    <div class="flex gap-4 mt-6 justify-center">
                        <button @click="filterTable"
                            class="flex py-2 px-3 gap-2 bg-sky-600 rounded-md text-center justify-center items-center hover:bg-green-600 duration-300">
                            <Icon icon="mynaui:search" width="20" height="24" />
                            <p>ค้นหา</p>
                        </button>

                        <button @click="clearFilter"
                            class="flex py-2 px-3 text-sky-500 border-sky-600 gap-2 border-2 bg-white rounded-md text-center hover:text-white hover:bg-red-600 hover:border-white duration-300">
                            <p>ล้างข้อมูล</p>
                        </button>
                    </div>
                </div>
            </div>
        </main>
    </div>
</template>

<script>
import { defineComponent, ref, watch } from "vue";
import { Icon } from '@iconify/vue';
import { NModal, NInput, NSelect, NDatePicker, NCheckbox, c } from "naive-ui";
import axios from "axios";
import { nextTick, computed } from 'vue';

import provinces from '../../static/provinces.json'
export default defineComponent({
    props: {
        dataTable: {
            type: Object,
            required: true
        }
    },
    components: {
        Icon,
        NModal,
        NInput,
        NSelect,
        NDatePicker,
        NCheckbox
    },
    setup(props, { emit }) {
        const localData = ref(props.dataTable);
        const patchData = ref({});
        let showModal = ref(false);
        const showPatchModal = ref(false);
        const submitSave = ref(false);
        const submitFailed = ref(false);

        const uploadedFiles = ref([]);
        const uploadedImage = ref(null);
        const fileInputRef = ref(null);

        const uploadedPatchFiles = ref([]);
        const uploadedPatchImage = ref(null);
        const fileInputPatchRef = ref(null);

        const defaultStatus = [
            { label: 'ทุกสถานะ', value: 'ทุกสถานะ' },
            { label: 'Active', value: true },
            { label: 'Inactive', value: false }
        ]
        const addSpecialRegistration = ref({
            province: 'กรุงเทพมหานคร',
            status: false
        });
        const searchKeyword = ref({
            letter: '',
            numberId: null,
            province: 'ทุกจังหวัด',
            groupSpecialRegistration: 'Black List',
            agency: '',
            status: 'ทุกสถานะ'
        })

        watch(() => props.dataTable, (newVal) => {
            localData.value = newVal;
        }, { deep: true });
        const fetchRegistrationsById = async (id) => {
            try {
                const response = await axios.get(`http://localhost:4000/registrations/${id}`);
                console.log('Registration fetched successfully:', response.data);
                patchData.value = response.data;
                showPatchModal.value = true;
                uploadedPatchImage.value = response.data.images.map(img => img.url);
                console.log('patchData', uploadedPatchImage.value);
                console.log('patchData', patchData.value);
            } catch (error) {
                console.error('Error fetching registration:', error);
            }
        };
        const deleteRegistrations = async (id) => {
            try {
                await axios.delete(`http://localhost:4000/registrations/${id}`);
            } catch (error) {
                console.error('Error deleting registration:', error);
            }
        };
        const deleteRow = (index, id) => {
            localData.value.row.splice(index, 1);
            emit('update:dataTable', localData.value);
            deleteRegistrations(id);
        };
        const postRegistrations = async () => {
            try {
                const formData = new FormData();
                uploadedFiles.value.forEach(file => {
                    console.log(file)
                    formData.append('files', file.file);
                    formData.append('uploadTime', file.uploadTime);
                });
                formData.append('letter', addSpecialRegistration.value.letter);
                formData.append('province', addSpecialRegistration.value.province);
                formData.append('numberId', addSpecialRegistration.value.numberId);
                formData.append('groupSpecialRegistration', addSpecialRegistration.value.groupSpecialRegistration);
                formData.append('ownUser', addSpecialRegistration.value.ownUser);
                formData.append('agency', addSpecialRegistration.value.agency);
                formData.append('status', addSpecialRegistration.value.status);
                formData.append('addDate', formatPostDatePicker(addSpecialRegistration.value.addDate));
                formData.append('fixDate', formatPostDatePicker(addSpecialRegistration.value.fixDate));
                formData.append('caseId', addSpecialRegistration.value.caseId);
                formData.append('behavior', addSpecialRegistration.value.behavior);
                formData.append('action', '');
                formData.append('phoneNumber', addSpecialRegistration.value.phoneNumber);
                console.log('Form Data Entries:', ref(addSpecialRegistration.value.status));

                console.log('addSpecialRegistration', addSpecialRegistration.value);
                const response = await axios.post('http://localhost:4000/registrations', formData, {
                    headers: {
                        'Content-Type': 'multipart/form-data',
                    },
                });
                const mapData = {
                    ...response.data,
                    numberId: response.data.letter + ' ' + response.data.numberId + ' ' + response.data.province,
                    addDate: formatDatePicker(response.data.addDate),
                    fixDate: formatDatePicker(response.data.fixDate),
                };

                console.log('mapData', mapData);
                localData.value.row.push(mapData);
                nextTick(() => {
                    console.log('DOM updated');
                });
            } catch (error) {
                console.error('Error posting registration:', error);
                console.log('Error ', response.data);
            }
        };
        const cancelCallback = () => {
            showModal.value = false;
            showPatchModal.value = false;
        }
        const submitCallback = () => {
            const newRegistration = {
                numberId: addSpecialRegistration.value.letter + ' ' + addSpecialRegistration.value.numberId + ' ' + addSpecialRegistration.value.province,
                list: addSpecialRegistration.value.groupSpecialRegistration,
                province: addSpecialRegistration.value.province,
                ownUser: addSpecialRegistration.value.ownUser,
                agency: addSpecialRegistration.value.agency,
                status: addSpecialRegistration.value.status === 'ทุกสถานะ' ? null : addSpecialRegistration.value.status ? true : false,
                addDate: formatDatePicker(addSpecialRegistration.value.addDate),
                fixDate: formatDatePicker(addSpecialRegistration.value.fixDate),
                image: uploadedFiles.value.map(file => ({
                    url: URL.createObjectURL(file.file),
                    alt: file.name
                }))
            };

            const hasUndefined = Object.values(newRegistration).some(value => value === undefined);
            const hasImages = newRegistration.image.length > 0;

            if (!hasUndefined && hasImages) {
                console.log('New Registration:', newRegistration);
                postRegistrations()

                uploadedFiles.value = [];
                uploadedImage.value = null;
                showModal.value = false;
                addSpecialRegistration.value = {
                    province: 'กรุงเทพมหานคร',
                    status: false
                };
                submitSave.value = true;
            } else {
                submitFailed.value = true;
            }
        };

        const summitPatch = async (id) => {

            try {
                const formData = new FormData();
                uploadedFiles.value.forEach(file => {
                    console.log(file)
                    formData.append('files', file.file);
                });
                formData.append('letter', patchData.value.letter);
                formData.append('province', patchData.value.province);
                formData.append('numberId', patchData.value.numberId);
                formData.append('groupSpecialRegistration', patchData.value.groupSpecialRegistration);
                formData.append('ownUser', patchData.value.ownUser);
                formData.append('agency', patchData.value.agency);
                formData.append('status', patchData.value.status);
                formData.append('addDate', formatPostDatePicker(patchData.value.addDate));
                formData.append('fixDate', formatPostDatePicker(patchData.value.fixDate));
                formData.append('caseId', patchData.value.caseId);
                formData.append('behavior', patchData.value.behavior);
                formData.append('action', '');
                console.log('patchData', formData);
                const response = await axios.patch(`http://localhost:4000/registrations/${id}`, formData, {
                    headers: {
                        'Content-Type': 'multipart/form-data',
                    },
                });
                const mapData = {
                    ...response.data,
                    numberId: response.data.letter + ' ' + response.data.numberId + ' ' + response.data.province,
                    addDate: formatDatePicker(response.data.addDate),
                    fixDate: formatDatePicker(response.data.fixDate),
                };

                console.log('mapData', mapData);

                const index = localData.value.row.findIndex(row => row.id === mapData.id);

                if (index !== -1) {
                    localData.value.row[index] = mapData;
                    showPatchModal.value = false;
                    submitSave.value = true;
                }
                showPatchModal.value = false;
                submitSave.value = true;
                nextTick(() => {
                    console.log('DOM updated');
                });
            } catch (error) {
                submitFailed.value = true;
                console.error('Error posting registration:', error);
            }
        }
        const triggerFileInput = () => {
            fileInputRef.value.click();
        };

        const handleFileUpload = (event) => {
            const files = event.target.files;
            let foundImage = false;
            console.log(uploadedFiles.value)
            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                uploadedFiles.value.push({
                    name: file.name,
                    uploadTime: formatDate(),
                    file: file
                });

                const imageExtensions = /\.(jpg|jpeg|png)$/i;
                if (!foundImage && imageExtensions.test(file.name)) {

                    const reader = new FileReader();
                    reader.onload = (e) => {
                        uploadedImage.value = e.target.result;
                    };
                    reader.readAsDataURL(file);
                    foundImage = true;
                }
            }

            if (!foundImage) {
                uploadedImage.value = null;
            }
        };

        const triggerPatchFileInput = () => {
            fileInputPatchRef.value.click();
        };

        const handlePatchFileUpload = (event) => {
            const files = event.target.files;
            let foundImage = false;
            console.log(uploadedPatchFiles.value);
            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                uploadedPatchFiles.value.push({
                    name: file.name,
                    file: {
                        ...file,
                        uploadTime: formatDate()
                    }
                });

                const imageExtensions = /\.(jpg|jpeg|png)$/i;
                if (!foundImage && imageExtensions.test(file.name)) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        uploadedPatchImage.value = e.target.result;
                    };
                    reader.readAsDataURL(file);
                    foundImage = true;
                }
            }

            if (!foundImage) {
                uploadedPatchImage.value = null;
            }
        };

        const filterTable = async () => {
            try {
                const searchParams = { ...searchKeyword.value };
                if (searchParams.status === 'ทุกสถานะ') {
                    searchParams.status = null;
                }
                if (searchParams.province === 'ทุกจังหวัด') {
                    searchParams.province = '';
                }

                const response = await axios.post('http://localhost:4000/registrations/filter', searchParams);
                console.log('response', response.data);
                localData.value.row = response.data.map(item => {
                    return {
                        ...item,
                        numberId: item.letter + ' ' + item.numberId + ' ' + item.province,
                        addDate: formatDatePicker(item.addDate),
                        fixDate: formatDatePicker(item.fixDate),
                    };
                });
                console.log('Registration posted successfully:', response.data);
            } catch (error) {
                console.error('Error posting registration:', error);
            }
        };
        const formatDate = () => {
            const date = new Date();
            const localTime = new Date(date.getTime());

            const day = localTime.getDate().toString().padStart(2, '0');
            const month = (localTime.getMonth() + 1).toString().padStart(2, '0');
            const year = (localTime.getFullYear() + 543);

            const hours = localTime.getHours().toString().padStart(2, '0');
            const minutes = localTime.getMinutes().toString().padStart(2, '0');

            return `${day}/${month}/${year} (${hours}:${minutes})`;
        };

        function formatDatePicker(timestamp) {
            const date = new Date(timestamp);
            const day = String(date.getDate()).padStart(2, '0'); // Get day and pad with zero if needed
            const month = String(date.getMonth() + 1).padStart(2, '0'); // Get month (0-11) and pad
            const year = date.getFullYear(); // Get full year

            return `${day}/${month}/${year}`; // Return in DD/MM/YYYY format
        }

        function clearFilter() {
            searchKeyword.value = {
                letter: '',
                numberId: null,
                province: 'ทุกจังหวัด',
                groupSpecialRegistration: '',
                agency: '',
                status: 'ทุกสถานะ'
            }
            filterTable()
        }
        function formatPostDatePicker(timestamp) {
            const date = new Date(timestamp);
            const day = String(date.getDate()).padStart(2, '0'); // Get day and pad with zero if needed
            const month = String(date.getMonth() + 1).padStart(2, '0'); // Get month (0-11) and pad
            const year = date.getFullYear(); // Get full year

            return `${year}/${month}/${day}`; // Return in DD/MM/YYYY format
        }

        const deleteFile = (fileName) => {
            const updatedFiles = uploadedFiles.value.filter(file => file.name !== fileName);
            uploadedFiles.value = updatedFiles;

            const imageExtensions = /\.(jpg|jpeg|png)$/i;
            const imageFiles = updatedFiles.filter(file => imageExtensions.test(file.name));

            if (imageFiles.length > 0) {
                const lastImageFile = imageFiles[imageFiles.length - 1].file;
                const reader = new FileReader();
                reader.onload = (e) => {
                    uploadedImage.value = e.target.result;
                };
                reader.readAsDataURL(lastImageFile);
            } else {
                uploadedImage.value = null;
            }
        };

        const deletePatchFile = (fileName) => {
            console.log('uploadedPatchImage', uploadedPatchFiles.value);
            const updatedFiles = uploadedPatchFiles.value.filter(file => file.name !== fileName);

            uploadedPatchFiles.value = updatedFiles;

            // กรองไฟล์ที่ไม่ตรงกับ fileName ออกจาก patchData.value.images
            patchData.value.images = patchData.value.images.filter(file => file.url !== fileName);

            // รวมไฟล์จาก patchData.images และ updatedFiles
            const allFiles = [...updatedFiles, ...patchData.value.images];

            // กรองเฉพาะไฟล์รูปภาพ
            const imageExtensions = /\.(jpg|jpeg|png)$/i;
            const imageFiles = allFiles.filter(file => imageExtensions.test(file.name || file.url));

            if (imageFiles.length > 0) {
                const lastImageFile = imageFiles[imageFiles.length - 1].file || imageFiles[imageFiles.length - 1].url;
                const reader = new FileReader();
                reader.onload = (e) => {
                    uploadedPatchImage.value = e.target.result;
                };

                if (lastImageFile instanceof File) {
                    // ถ้าเป็นไฟล์ใหม่ ให้ใช้ readAsDataURL
                    reader.readAsDataURL(lastImageFile);
                } else {
                    // ถ้าเป็น URL ที่มีอยู่แล้ว
                    uploadedPatchImage.value = lastImageFile;
                }
            } else {
                uploadedPatchImage.value = null;
            }
        };

        const lastImage = computed(() => {

            uploadedPatchImage.value;
            if (patchData.value.images.length > 0) {
                uploadedPatchImage.value;
                return patchData.value.images[patchData.value.images.length - 1].url;
            }
            console.log('xxx', lastImage);
            return null;
        });
        const formattedImageUrl = (url) => {
            return url.split('/').pop();
        }
        return {
            localData,
            deleteRow,
            cancelCallback,
            uploadedFiles,
            handleFileUpload,
            submitCallback,
            showModal,
            triggerFileInput,
            fileInputRef,
            deleteFile,
            uploadedImage,
            addSpecialRegistration,
            provinces,
            submitSave,
            submitFailed,
            searchKeyword,
            defaultStatus,
            deleteRegistrations,
            filterTable,
            clearFilter,
            fetchRegistrationsById,
            showPatchModal,
            patchData,
            uploadedPatchFiles,
            uploadedPatchImage,
            fileInputPatchRef,
            triggerPatchFileInput,
            handlePatchFileUpload,
            formattedImageUrl,
            deletePatchFile,
            summitPatch,
            lastImage,
            groupSpecialRegistration: [
                { label: 'Black List', value: 'Black List' },
                { label: 'White List', value: 'White List' },
            ]
        };
    }
});
</script>
<style scoped>
/* Customize the scrollbar for Webkit browsers (Chrome, Safari) */
::-webkit-scrollbar {
    width: 8px;
    /* Set the width of the scrollbar */
    height: 8px;
    /* Set the height of the scrollbar */
    background: transparent;
    /* Optional: make the scrollbar background transparent */
}

/* Customize the scrollbar thumb for Webkit browsers */
::-webkit-scrollbar-thumb {
    background: #888;
    /* Set the color of the scrollbar thumb */
    border-radius: 4px;
    /* Add rounded corners to the scrollbar thumb */
}

/* Customize the scrollbar track for Webkit browsers */
::-webkit-scrollbar-track {
    background: #f1f1f1;
    /* Set the color of the scrollbar track */
}

/* For Firefox */
* {
    scrollbar-width: thin;
    /* Set the scrollbar width to thin */
    scrollbar-color: #888 #f1f1f1;
    /* Set the color of the scrollbar thumb and track */
}

/* For Internet Explorer 10+ */
* {
    -ms-overflow-style: -ms-autohiding-scrollbar;
    /* Use auto-hiding scrollbar */
}
</style>
