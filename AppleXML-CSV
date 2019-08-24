import xml.etree.ElementTree as ET
import csv
import datetime

def convert_xml_to_csv(file_path):

    with open(file_path, 'rb') as fd:
        root = ET.parse(fd).getroot()
    
    records = []
    keys = None
    for record in root:
        if record.tag == 'Record':
            if not keys:
                keys = list(record.attrib.keys())
                #keys.append('value_c')   #for non-numeric value
            
            # convert record.attrib into normal dict to add some features later (from MetaReocrds etc)
            att_values = record.attrib

            # Get MetadataEntry
            for matt in root[2]:
                att_values[matt.attrib['key']] = matt.attrib['value']
                if matt.attrib['key'] not in keys:
                    keys.append(matt.attrib['key'])
            
            # keys could be different record by record (field "device")
            for key in att_values.keys():
                if key not in keys:
                    keys.append(key)
                    
            # final check for special feature 'Value'
            try:
                float(att_values['value'])
            except ValueError:
                #att_values['value_c'] = att_values['value']
                #att_values['value'] = 0
                continue
            
            # add to dictionary
            records.append(att_values)
        
    print("Attribute records were extracted from {}, found {} entries".format(file_path, len(records)))
    
    # Export file name : Just simply remove extension "xml" then add "csv"
    export_file_name = file_path.replace(".xml", datetime.datetime.now().strftime("%Y%m%d%H%M%S") + ".csv")
    
    with open(export_file_name, 'w') as outfile:
        dict_writer = csv.DictWriter(outfile, keys)
        dict_writer.writeheader()
        dict_writer.writerows(records)

    print("{} was saved as csv format".format(export_file_name))
    
if __name__ == '__main__':
    convert_xml_to_csv('tmp_files/export.xml')   #replace with your file name location
